pipeline {
    agent any

    environment {
        IMAGE = "vk0908/flask-app"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/vk0809/jenkins-project2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE:v1 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE:v1
                    '''
                }
            }
        }

    }
}
