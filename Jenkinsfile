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
        stage('Artifactory configuration') {
		
	   steps {
		script {
			rtMaven.tool = 'M2_HOME' //Maven tool name specified in Jenkins configuration
		
			rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server //Defining where the build artifacts should be deployed to
			
			rtMaven.resolver releaseRepo:'libs-release', snapshotRepo: 'libs-snapshot', server: server //Defining where Maven Build should download its dependencies from
			
			rtMaven.deployer.artifactDeploymentPatterns.addExclude("pom.xml") //Exclude artifacts from being deployed
			
			//rtMaven.deployer.deployArtifacts =false // Disable artifacts deployment during Maven run
		    
			buildInfo = Artifactory.newBuildInfo() //Publishing build-Info to artifactory
			
			buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true

			buildInfo.env.capture = true
			}
	    }
	}
     
    }
	}
