pipeline {
    agent any
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
                rtMavenRun (
                    tool: 'maven', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install'
                   
                )
            }
        }
		}
		}
