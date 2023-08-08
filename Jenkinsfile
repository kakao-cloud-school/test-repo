pipeline {
    agent any
    triggers {
        githubPush() // GitHub에 커밋이 발생하면 자동으로 파이프라인 시작
    }
    environment {
        REPOSITORY = "joyoungkyung/jenkinshub"
        DOCKERHUB_CREDENTIALS = credentials('docker_access_token')
    }
    stages {
        stage('Checkout') {
            steps {
                // GitHub 저장소의 코드를 가져옴
                checkout scm
            }
        }
      stage('Build Docker Image') {
          steps {
              script {
            docker.build("joyoungkyung/jenkinshub", ".")
                   }
               }
           }
        stage('Docker Login') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_access_token', variable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
                        sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    }
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    sh "docker push $REPOSITORY"
                }
            }
        }
        stage('Docker Pull') {
            steps {
                script {
                    sh "docker pull $REPOSITORY"
                }
            }
        }
    }
}
