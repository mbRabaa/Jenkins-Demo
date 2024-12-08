node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImage
    def dockerImageTag = "mbrabaa2023/newapp-image:${env.BUILD_NUMBER}"  // Modification ici : nouveau nom de l'image "newapp-image"
    def containerName = "newapp-container"  // Nouveau nom de conteneur "newapp-container"

    stage('Clone Repo') {
        git branch: 'main', url: 'https://github.com/wahid007/Jenkins-Demo.git'
    }

    stage('Build Project') {
        sh "mvn clean package"
    }

    stage('Initialize Docker') {
        def dockerHome = tool 'MyDocker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }

    stage('Build Docker Image') {
        sh "docker build -t ${dockerImageTag} ."
    }

    stage('Login to DockerHub') {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
        }
    }

    stage('Deploy Docker Image') {
        echo "Docker Image Tag Name: ${dockerImageTag}"
        // Utilisation du nouveau nom de conteneur
        sh "docker run --name ${containerName} -d -p 2222:2222 ${dockerImageTag}"
    }

    stage('Push Docker Image to DockerHub') {
        echo "Pushing Docker image to DockerHub"
        sh "docker push ${dockerImageTag}"
    }
}
 "modification de non d'image et de nom contenaire"
