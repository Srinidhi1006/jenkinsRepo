pipeline {
    agent any
 
    stages {
        stage('Checkout') {
            steps {
                // Check out code from the GitHub repository
                script {
                    def gitCredentials = credentials('Srinidhi1006') // Replace with your credentials ID
                    checkout([$class: 'GitSCM', 
                         branches: [[name: 'main']], 
                         userRemoteConfigs: [[url: 'https://github.com/Srinidhi1006/jenkinsRepo.git', credentialsId: gitCredentials]]
                }
            }
        }
 
        stage('Build') {
            steps {
                // Your build steps here
                sh 'mvn clean install' // Example Maven build command
            }
        }
 
        // Add more stages for testing, deployment, etc.
    }
}
