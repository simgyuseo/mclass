pipeline {
    agent any 
    
    tools {
        maven 'maven 3.9.12'
    } // <--- 이 닫는 괄호가 빠져있었습니다! 반드시 추가해야 합니다.

    environment {
        DOCKER_IMAGE = "demo-app"
        CONTAINER_NAME = "springboot-container"
        JAR_FILE_NAME = "app.jar"
        PORT = "8081"
        REMOTE_USER = "ec2-user"
        REMOTE_HOST = "13.209.166.210"
        REMOTE_DIR = "/home/ec2-user/deploy"
        SSH_CREDENTIALS_ID = "9cad3e28-19c5-4a40-935f-f9a8f44a0fa3"
    }

    stages {
        stage('Git checkout') {
            steps {
                checkout scm
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Prepare Jar') {
            steps {
                sh 'cp target/demo-0.0.1-SNAPSHOT.jar ${JAR_FILE_NAME}'
            }
        }
        stage('Copy to Remote Server') {
            steps {
                sshagent (credentials: [env.SSH_CREDENTIALS_ID]) {
                    sh "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ${REMOTE_USER}@${REMOTE_HOST} \"mkdir -p ${REMOTE_DIR}\""
                    sh "scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ${JAR_FILE_NAME} Dockerfile ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}/"
                }
            }
         }
        stage('Remote Docker Build & Deploy') {
            steps {
                sshagent (credentials: [env.SSH_CREDENTIALS_ID]) {
                    sh """
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null ${REMOTE_USER}@${REMOTE_HOST} << ENDSSH
    cd ${REMOTE_DIR} || exit 1
    docker rm -f ${CONTAINER_NAME} || true
    docker build -t ${DOCKER_IMAGE} .
    docker run -d --name ${CONTAINER_NAME} -p ${PORT}:${PORT} ${DOCKER_IMAGE}
ENDSSH
                    """
                }
            }
         }
    } //stage 블록을 닫는 괄호
} // pipeline 블록을 닫는 괄호