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
                nexusArtifactUploader artifacts: [[artifactId: 'maven-snapshots', classifier: '', file: '/var/lib/jenkins/workspace/testproject_pipeline/target/devops-petclinic-2.1.0.BUILD-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexus credential', groupId: 'nexus', nexusUrl: 'http://35.244.2.56:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0'
                
            }
        }
			}
		}
