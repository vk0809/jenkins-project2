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
        stage ('Deploy green') {
            steps {
                sh 'kubectl apply -f green-deployment.yml'
            }
        }
        stage('wait & verify') {
            steps {
                
                sh 'sleep 10'
                sh 'kubectl rollout status deployment/flask-green'
                sh 'kubectl get pods'
            }
        }
        stage('Switch Traffic to green') {
            steps {
                sh '''
                sed -i "s/version: blue/version: green/" service.yml
                kubectl apply -f service.yml
                '''
            }
        }

    }
}
