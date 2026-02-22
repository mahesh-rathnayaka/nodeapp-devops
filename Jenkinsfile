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
                sh 'docker build -t maheshur/nodeapp-test:$BUILD_NUMBER .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u maheshur --password-stdin
                    '''
                }
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push maheshur/nodeapp-test:${BUILD_NUMBER}'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
