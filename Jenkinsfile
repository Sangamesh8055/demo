pipeline {
    agent any

    environment {
        dockerRepo = 'sangaesh8055/demo'
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
                    sh 'docker build -t sangaesh8055/demo:${BUILD_NUMBER} .'
                }
            }
        } 
             
         stage('PUSH') {
            steps {
                script {
                    // Authenticate with Docker Hub using credential helper
                    withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh "echo \"${DOCKER_PASSWORD}\" | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    }

                    // Push the Docker image to Docker Hub
                    sh "docker push sangaesh8055/demo:${BUILD_NUMBER}"
                    sh "docker ps -a"
                    sh "docker images"
                }
            }
        }

        stage('Helm Deploy') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'kubeconfig-text', variable: 'KUBECONFIG_CONTENT')]) {
                        
                    sh """
                    echo "${KUBECONFIG_CONTENT}" > kubeconfig.yaml
                    export KUBECONFIG=kubeconfig.yaml
                    helm upgrade --install sang ./demo \\
                    --set image.repository=${dockerRepo} \\
                    --set image.tag=${BUILD_NUMBER}
                    """
                    }
                }
            }
        }

}
}
