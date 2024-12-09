pipeline {
    agent any
    environment {
        MONGO_URL = "mongodb+srv://charan03:030904@clusterecoverse.xxoya0d.mongodb.net/ecoverse"
        JWT_SECRET = "SDNCN23423TN394UFKND"
        jwt_token = "dhfmharnca394020"
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }
    stages {
        stage('Stage 1: Git Clone') {
            steps {
                git branch: 'main',
                url: 'https://github.com/chokshiyadav/spedemo.git'
            }
        }
        stage('client build') {
            steps {
                dir('client'){
                sh 'docker build -t chokshi/frontend:latest .'
            }
            }
        }
        stage("Server build") {
            steps {
                dir('backend'){
                sh 'docker build -t chokshi/backend:latest .'
            }}
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                        withCredentials([usernamePassword(credentialsId: 'DockerHubCred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                            sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                                            sh 'docker tag frontend-image chokshi/frontend-image:latest'
                                            sh 'docker push chokshi/frontend-image:latest'
                                            sh 'docker tag backend-image chokshi/backend-image:latest'
                                            sh 'docker push chokshi/backend-image:latest'
                        }
                    
                }
            }
        }
        stage('Ansible Deployment') {
            steps {
                script { 
                    sh 'ansible-playbook playbook.yml -i inventory'
                }
            }
        }
    }
}