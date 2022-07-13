pipeline {
    agent {label 'worker001'}
    environment {
    DOCKERHUB_CREDENTIALS = credentials('sayedshalabi')
    }
    stages { 
        stage('Build docker image') {
            steps {  
                sh 'docker build -t sayedshalabi/nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push sayedshalabi/nodeapp:$BUILD_NUMBER'
            }
        }

        stage('Deploy') {
            steps{
                sh 'ssh server@192.168.0.123 kubectl create -f /nodeapp/deployment.yml'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}

