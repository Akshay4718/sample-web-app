pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Akshay4718/sample-web-app.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-web-app:latest .'
            }
        }
        stage('Tag Image') {
            steps {
                sh 'docker tag sample-web-app:latest akshay1867/sample-web-app:latest'
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push akshay1867/sample-web-app:latest'
                }
            }
        }
        stage('Pull Image from Docker Hub') {
            steps {
                sh 'docker pull akshay1867/sample-web-app:latest'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f sample-web-app || true
                docker run -d -p 80:80 --name sample-web-app akshay1867/sample-web-app:latest
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
