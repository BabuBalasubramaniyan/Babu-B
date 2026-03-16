pipeline {
    agent any

    environment {
        DEV_REPO="dockerhubusername/dev"
        PROD_REPO="dockerhubusername/prod"
        IMAGE_NAME="react-devops"
    }

    stages {

        stage('Clone Code') {
            steps {
                git url: 'https://github.com/<username>/devops-build.git',
                branch: "${env.BRANCH_NAME}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
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

        stage('Push to DEV Repo') {
            when {
                branch 'dev'
            }
            steps {
                sh '''
                docker tag $IMAGE_NAME $DEV_REPO:latest
                docker push $DEV_REPO:latest
                '''
            }
        }

        stage('Push to PROD Repo') {
            when {
                branch 'master'
            }
            steps {
                sh '''
                docker tag $IMAGE_NAME $PROD_REPO:latest
                docker push $PROD_REPO:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop react-app || true
                docker rm react-app || true
                docker run -d -p 80:80 --name react-app $IMAGE_NAME
                '''
            }
        }

    }
}
