pipeline {
    agent any
    tools {
    maven 'M2_HOME'
	def server
    def buildInfo
    def rtMaven
  }    
    
	environment {
     sonar_url = "http://sonalrul:9000"
	 Artifactory_url = "http://35.244.57.13:8081"
   }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn install' 
            }
            
        }
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
		}
