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
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'sudo docker push dockernaan2/nodeapp:$BUILD_NUMBER'
            }
        }
        stage('K8S Deploy') {
        steps{   
            script {
                withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                sh 'kubectl apply -f  deploy.yaml'
                }
            }
        }
       }
}
post {
        always{
            sh 'docker logout'
        }
    }
}
