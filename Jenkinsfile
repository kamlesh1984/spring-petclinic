node {
    def server
    def buildInfo
    def rtMaven

    stage ('Clone') {
        git url: 'https://github.com/kamlesh1984/spring-petclinic.git'
    }

    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
        server = Artifactory.server Artifactory

        rtMaven = Artifactory.newMavenBuild()
        rtMaven.tool = M2_HOME // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run

        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Test') {
        rtMaven.run pom: 'spring-petclinic/pom.xml', goals: 'clean test'
    }

    stage ('Install') {
        rtMaven.run pom: 'spring-petclinic/pom.xml', goals: 'install', buildInfo: buildInfo
    }

    stage ('Deploy') {
        rtMaven.deployer.deployArtifacts buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
} 
