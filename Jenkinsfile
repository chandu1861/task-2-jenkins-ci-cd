pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/chandu1861/CI-CD-Project.git'
            }
        }

        stage('Build and tag') {
            steps {
                sh "docker build -t chandana1213/img:v1 ."
            }
        }

        stage('Containersation') {
            steps {
                sh '''
                    docker run -it -d --name c3 -p 9010:8000 chandana1213/img:v1
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
                sh "docker push chandana1213/img:v1"
            }
        }
        stage('Deploy to kubernetes') {
            steps {
                script {
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubernetes', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                    sh 'kubectl delete --all pods'
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
            }
        }
    }
  }
}
}
