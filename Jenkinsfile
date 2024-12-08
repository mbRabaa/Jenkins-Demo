pipeline {
    agent any

    environment {
        registry = "wahidh007/demo-jenkins"
        registryCredential = 'docker-hub-credentials'
        dockerImage = ''        
    }

    triggers {
        pollSCM('*/5 * * * *')
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/wahid007/Jenkins-Demo.git'
            }
        }
        
        stage('Build App') {
            steps {
                sh "mvn clean package"
            }
        }
        
        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"   
                }          
            }
        }

        stage('Deploy Docker container'){
            steps {
                sh "docker run --name demo-jenkins -d -p 2222:2222 $registry:$BUILD_NUMBER"
                // Slack notification is removed or commented out
                // slackSend color: "good", message: registry + ":$BUILD_NUMBER" + " - image successfully created! :man_dancing:"
            }
        }

    }

    post {
        success {
            echo 'Pipeline execution successful!'
            // Slack notification is removed or commented out
            // slackSend color: "good", message: "Pipeline execution successful! :man_dancing:"
        }
        failure {
            echo 'Pipeline execution failed.'
            // Slack notification is removed or commented out
            // slackSend color: "danger", message: "Pipeline execution failed! :ghost:"
        }
    }    
}
