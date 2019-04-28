pipeline {
    agent any
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
        stage ('Publish on Artifactory') {
            steps {
                rtServer (
                    id: "ART",
                    url: "http://35.244.2.56:8081/artifactory",
                    credentialsId: "Artifactory"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ART",
                    releaseRepo: "petclinic",
                    snapshotRepo: "petclinic"
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
