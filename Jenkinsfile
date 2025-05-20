pipeline {
    agent any
    
    environment {
        registryCredential = 'docker'
    }

    stages {
        stage('GIT SCM') {
            steps {
             git 'https://github.com/Sangamesh8055/demo.git'   
            }
        }
        stage(BUILD) {
            steps {
                script {
                    sh 'docker build -t sangamesh8055/demo:${BUILD_NUMBER} .'
                }
            }
        } 
             
         stage('PUSH') {
            steps {
                script {
                    // Authenticate with Docker Hub using credential helper
                    withCredentials([usernamePassword(credentialsId: "${registryCredential}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh "echo \"${DOCKER_PASSWORD}\" | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    }

                    // Push the Docker image to Docker Hub
                    sh "docker push sangamesh8055/demo:${BUILD_NUMBER}"
                    sh "docker ps -a"
                    sh "docker images"
                }
            }
        }

}
}
