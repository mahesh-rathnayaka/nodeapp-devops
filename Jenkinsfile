pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/mahesh-rathnayaka/nodeapp-devops.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                sh 'docker build -t maheshur/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test-dockerhubpass')]) {
                    script {
                        sh "docker login -u maheshur -p %test-dockerhubpass%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push maheshur/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
