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
    }
} // pipeline 블록을 닫는 괄호