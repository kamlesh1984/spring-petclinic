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
        
		stage ('Upload on Nexus') {
            steps {
                nexusArtifactUploader credentialsId: 'nexus-cred', groupId: 'Nexus', nexusUrl: '35.244.2.56:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'petclinic-sanpshot', version: '1.0'
                
            }
        }
			}
		}
