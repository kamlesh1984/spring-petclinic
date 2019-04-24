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
				}
				}
