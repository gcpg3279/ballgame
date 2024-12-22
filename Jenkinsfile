pipeline {
    agent { label 'gke-server' }
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/gcpg3279/ballgame.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t gameimage /home/chanelgoofy/workspace/game-project-1'
                sh 'sudo docker tag gameimage vijay3639/gameimage:latest'
                sh 'sudo docker tag gameimage vijay3639/gameimage:${BUILD_NUMBER}'
            }
        }
        stage('Trivy Vulnerability Scan') {
            steps {
                script {
                    sh 'sudo trivy image vijay3639/gameimage:latest'
                    sh 'sudo trivy image vijay3639/gameimage:${BUILD_NUMBER}'
                }
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push vijay3639/gameimage:latest'
                sh 'sudo docker image push vijay3639/gameimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /home/chanelgoofy/workspace/game-project-1/pod.yml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
