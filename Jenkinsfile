pipeline {
    agent any
    environment {
        IMAGE_NAME = "ssanik/hello-devops"
        IMAGE_TAG = "${BUILD_NUMBER}" 
        FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"

        SSH_CREDENTIALS = 'deploy-server-ssh'
        REMOTE_SERVER_IP = '192.168.56.11'
        DOCKER_HUB_CREDENTIALS = 'docker-hub-creds'
    }
    stages {
        stage('Git pull') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SadmanSakibAnik/hello-devops.git'
            }
        }
        stage('Prepare Dockerfile') {
            steps {
                // Copy Dockerfile from /opt to workspace
                sh "cp /opt/Dockerfile ."
            }
        }
        stage('Build docker image') {
            steps {
                sh "docker build -t ${FULL_IMAGE} ."
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }
        stage('Push image to Docker Hub') {
            steps {
                sh "docker push ${FULL_IMAGE}"
            }
        }
        stage('Deploy on Remote Server') {
            steps {
                sshagent(credentials: [SSH_CREDENTIALS]) {
                    sh "ssh -o StrictHostKeyChecking=no vagrant@${REMOTE_SERVER_IP} 'docker pull ${FULL_IMAGE}'"
                    sh "ssh -o StrictHostKeyChecking=no vagrant@${REMOTE_SERVER_IP} 'docker rm -f hello-devops || true'"
                    sh "ssh -o StrictHostKeyChecking=no vagrant@${REMOTE_SERVER_IP} 'docker run -d -p 3000:3000 --name hello-devops --restart always ${FULL_IMAGE}'"
                }
            }
        }
    }
}
