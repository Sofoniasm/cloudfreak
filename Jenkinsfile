pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'JDK8'
    }
    stages {
        stage('Build Maven') {
            steps {
                sh 'pwd'
                sh 'mvn clean package'
            }
        }

        stage('Copy Artifact') {
            steps {
                sh 'pwd'
                sh 'cp -r target/*.jar docker/'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def customImage = docker.build('sofoniasm/cloudfreak', '-f docker/Dockerfile .')
                    docker.withRegistry('https://registry.hub.docker.com', 'Docker-Hub') {
                        customImage.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
    }
}
