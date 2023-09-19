pipeline {
    agent any
 
    stages {
	
        stage('Checkout') {
            steps {
                // Check out code from the GitHub repository
                script {
                    def gitCredentials = credentials('Srinidhi1006') // Replace with your credentials ID
                    checkout([$class: 'GitSCM', 
                         branches: [[name: 'master']], 
                         userRemoteConfigs: [[url: 'https://github.com/Srinidhi1006/jenkinsRepo.git', Srinidhi1006: gitCredentials]]])
                }
            }
        }
 
       stage("Build in Maven"){
	   steps {

            echo "Enter Build in Maven"


         bat (returnStdout: true, script: "cd C:/ProgramData/Jenkins/.jenkins/workspace/Hello_master && mvn clean install -Dmaven.test.skip=true || echo success")

          echo "Exit Build Maven"


        }
		}
 
        // Add more stages for testing, deployment, etc.
    }
}
