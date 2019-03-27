pipeline {
    agent any
    tools {
    maven 'M2_HOME'
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
		}
