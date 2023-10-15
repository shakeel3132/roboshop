pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', changelog: false, poll: false, url: 'https://github.com/shakeel3132/roboshop.git'
                //checkout scm
            }
        }
        
        stage('Build and Push Docker Images') {
            steps {
                script {
               withCredentials([usernameColonPassword(credentialsId: 'jfrog-cred', variable: 'jfrog-cred')]) {
                    // Define a list of Docker image names
                    def dockerImages = ['web', 'mongodb', 'catalogue', 'user', 'cart', 'mysql', 'shipping', 'ratings', 'payment']
                    def artifactoryRepository = "172.31.44.226:8081/docker-local"
                    def artifactoryCredentials = credentials('jfrog-cred')
                    for (imageName in dockerImages) {
                     
                        sh "cd ${imageName} && docker build -t ${imageName}:v1 . && cd .."
                        sh "docker tag ${imageName}:v1 ${artifactoryRepository}/${imageName}:v1"
                        sh "docker push ${artifactoryRepository}/${imageName}:v1"
                    }
                    }    
                }
            }
        }
   
          stage('Deploy') {
            steps {
                sh '''
                 mkdir -p \$WORKSPACE/mysql \$WORKSPACE/rabbitmq \$WORKSPACE/redis \$WORKSPACE/mongodb
                  docker compose up -d
                '''
            }
        }

    }
}






