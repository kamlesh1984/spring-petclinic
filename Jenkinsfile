pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/kamlesh1984/spring-petclinic.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "Artifactory",
                    url: "http://35.244.57.13:8081/artifactory",
                    credentialsId: "Artifactory"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: M2_HOME, // Tool name from Jenkins configuration
                    pom: 'spring-petclinic/pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }
    }
}
