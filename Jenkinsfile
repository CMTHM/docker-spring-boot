pipeline {
    agent any

    environment {
        registry = "014498658637.dkr.ecr.ap-south-1.amazonaws.com/demo/springboot-app"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/CMTHM/docker-spring-boot.git']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 014498658637.dkr.ecr.ap-south-1.amazonaws.com"
                    sh "docker push 014498658637.dkr.ecr.ap-south-1.amazonaws.com/demo/springboot-app:latest"
                    
                }
            }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package springboot"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade mycharts springboot-0.1.0.tgz"
                }
            }
    }
}