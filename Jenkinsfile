pipeline {
    agent any
    tools {
    maven 'M2_HOME'
  }    
    
	environment {
     sonar_url = "http://sonalrul:9000"
	 Artifactory_url = "http://nexusurl:8081"
	 dev_url = "http://dev_url:port"
	 test_url = "https://test_url:port"
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
                sh 'mvn install -Dbuildversion=1.0.0-DEV${BUILD_NUMBER}' 
            }
            
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deploy.sh'
            }
        }
    }
	}
