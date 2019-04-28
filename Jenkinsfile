pipeline {
    agent any
    def server = Artifactory.newServer url: http://35.244.2.56:8081/artifactory, credentialsId: Artifactory
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
	tools {
        maven 'maven'
    }
    environment {
     sonar_url = "http://35.200.225.134:9000/sonar/"
    }	 
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/kamlesh1984/spring-petclinic.git"
            }
        }
		stage ('Build') {
            steps {
                sh 'mvn install' 
				}
			            }
		stage ('Sonar_Petclinic') {
			steps {
			 sh 'mvn sonar:sonar -Dsonar.host.url=http://35.200.202.127:9000 -Dsonar.login=7ca4c46a9982dd07bd22e584d2802c6ad851e70b -Dsonar.projectKey=spring-petclinic -Dsonar.branch=Petclinic'
			}
			}
        stage ('Artifactory configuration') {
        rtMaven.tool = MAVEN_TOOL // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    

        stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
			}
		}
