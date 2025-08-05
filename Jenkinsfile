pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/chandu1861/task-2-jenkins-ci-cd.git'
            }
        }

        stage('Build and tag') {
            steps {
                sh "docker build -t chandana1213/coffee:latest ."
            }
        }

        stage('Containersation') {
            steps {
                sh '''
                    docker run -it -d --name c3 -p 9010:8000 chandana1213/coffee:latest
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }

        stage('Pushing image to repository') {
            steps {
                sh "docker push chandana1213/coffee:latest"
            }
        }  
  }
}
}
