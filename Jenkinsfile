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
      stage('Publish') {
      def server = Artifactory.server 'Artifactory'
      def rtMaven = Artifactory.newMavenBuild()
      rtMaven.tool = 'M2_HOME'
      rtMaven.resolver server: server, releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot'
      rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
      rtMaven.deployer.artifactDeploymentPatterns.addInclude("*stubs*")
      def buildInfo = rtMaven.run pom: 'person-service/pom.xml', goals: 'clean install'
      rtMaven.deployer.deployArtifacts buildInfo
      server.publishBuildInfo buildInfo
     
    }
	}
