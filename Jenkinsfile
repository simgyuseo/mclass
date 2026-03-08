pipeline {
    agent any // 모든 서버에서 실행 가능
    
    tools {
        maven 'maven 3.9.12'
        //Jenkins에 등록된 Maven 사용
    
    environment {
        // 배포에 필요한 변수 설정
        DOCKER_IMAGE = "demo-app" //도커 이미지 이름
        CONTAINER_NAME = "springboot-container" //도커 컨테이너 이름
        JAR_FILE_NAME = "app.jar" //복사할 jar 파일 이름
        PORT = "8081" //컨테이너와 연결할 포트
        REMOTE_USER = "ec2-user" //원격(SPRING) 서버 사용자
        REMOTE_HOST = "13.209.166.210" //원격(spring) 서버 ip 
        REMOTE_DIR = "/home/ec2-user/deploy" //원격 서버에 파일 복사할 경로 
        SSH_CREDENTIALS_ID= "9cad3e28-19c5-4a40-935f-f9a8f44a0fa3"

    }

    //여러 단계를 그룹화
    stages {
        stage('Git checkout') {
            steps { //step : stage 안에서 실행할 실제 명령어
                // Jenkins가 연결된 git 저장소에서 최신 코드 체크아웃
                checkout scm
            }
        }
        stage('Maven Build') {
            steps {
                //테스트는 건너뛰고 Maven 빌드 
                sh 'mvn clean package -DskipTests'
                //sh : 리눅스 명령어 실행 
            }
        }
    }

    }
}