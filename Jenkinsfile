pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t babubalasubramaniyan/react-devops:latest .'
            }
        }

        stage('Login DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push babubalasubramaniyan/react-devops:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 3000:3000 babubalasubramaniyan/react-devops:latest'
            }
        }

    }
}
