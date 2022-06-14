pipeline {
    agent any
    environment {
    DOCKERHUB_CREDENTIALS = credentials('dockernaan2')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git 'https://github.com/sharath2408/nodejs-demo.git'
            }
        }

        stage('Build docker image') {
            steps{  
                sh 'sudo docker build -t dockernaan2/nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'sudo echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'sudo docker push dockernaan2/nodeapp:$BUILD_NUMBER'
            }
        }   
        stage('kubernetes deployment'){
            steps{
                 sh 'kubectl apply -f deploy.yml'
            }
        }
}
post {
        always{
            sh 'docker logout'
        }
    }
}
