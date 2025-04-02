pipeline {
    agent any  

    environment {
        IMAGE_NAME = 'matougong/tinode'
        IMAGE_TAG = 'latest'
        DOCKER_HUB_CREDENTIALS = 'dockerhub' // ID des credentials dans Jenkins
    }

    stages {

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

      stage('Login to Docker Hub') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_HUB_USR', passwordVariable: 'DOCKER_HUB_PSW')]) {
            sh 'echo $DOCKER_HUB_PSW | docker login -u $DOCKER_HUB_USR --password-stdin'
            sh 'docker push matougong/tinode'
        }
    }
}


        stage('Push Image to Docker Hub') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker stop mon-projet || true && docker rm mon-projet || true"
                    sh "docker run -d -p 3000:3000 --name mon-projet ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé'
        }
    }
}
