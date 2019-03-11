pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk8'
    }
	environment {
     sonar_url = "http://sonalrul:9000"
	 Artifactory_url = "http://nexusurl:8081"
	 dev_url = "http://dev_url:port"
	 test_url = "https://test_url:port"
   }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
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
