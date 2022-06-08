


pipeline {
     agent none
  
    environment {
    DOCKERHUB_CREDENTIALS = credentials('dockernaan2')
    }
    stages { 
        stage('SCM Checkout') {
            agent {
                label "slave-node"
            } 
            
           steps{
            git 'https://github.com/sharath2408/nodejs-demo.git'
            }
        }

        stage('Build docker image') {
            steps {  
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
        stage('Deploy App') {
      steps {
          kubernetesDeploy(configs: "deploy.yml", kubeconfigId: "KUBERNETES_CLUSTER_CONFIG")
      }
    }
}
post {
        always {
            sh 'docker logout'
        }
    }
}
