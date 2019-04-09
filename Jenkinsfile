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
                    tool: 'M3', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install'
                   
                )
            }
        }
		

        stage ('Publish on Artifactory') {
            steps {
                rtServer (
                    id: "ART",
                    url: "http://35.244.56.18:8081/artifactory",
                    credentialsId: "Artifactory"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ART",
                    releaseRepo: "example-repo-local",
                    snapshotRepo: "example-repo-local"
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
		stage ('Sonar_JAVA') {
			steps {
			    
			    sh 'mvn -f /var/lib/jenkins/workspace/spring-petclinic/pom.xml -e -B sonarqube:sonarqube  -Dsonar.language=java -Dsonar.sources="./" -Dsonar.inclusions="**/*.java" -Dsonar.host.url="${sonar_url}" -Dsonar.login="admin" -Dsonar.password="admin" -Dsonar.projectKey=petclinic -Dsonar.branch=JAVABranch -Dbuildversion=1.0.0-DEV01'
                }
            }
    }
}
