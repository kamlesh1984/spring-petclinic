pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/kamlesh1984/spring-petclinic.git"
            }
        }
		stage ('Build') {
            steps {
                rtMavenRun (
                    tool: 'M3', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install'
                   
                )
            }
        }
		

        stage ('Publish on Artifactory') {
            steps {
                rtServer (
                    id: "ART",
                    url: "http://35.244.57.13:8081/artifactory",
                    credentialsId: "Artifactory"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ART",
                    releaseRepo: "example-repo-local",
                    snapshotRepo: "example-repo-local"
                )
            }
        }

    

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ART"
                )
            }
        }
    }
}
