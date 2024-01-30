pipeline {
    agent any

    stages {
        stage('clone') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/venkatkukatla/ecrtoeks.git']])
            }
        }
        
        stage('ecr login') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 613980700205.dkr.ecr.us-east-1.amazonaws.com'
            }
        }
        
        stage('docker build') {
            steps {
                sh 'docker build -t nginxecr .'
            }
        }
        
        stage('Tag') {
            steps {
                sh 'docker tag nginxecr:latest 613980700205.dkr.ecr.us-east-1.amazonaws.com/nginxecr:latest'
            }
        }
        
        stage('Docker push'){
            steps {
                sh 'docker push 613980700205.dkr.ecr.us-east-1.amazonaws.com/nginxecr:latest'
            }
        }
        
        stage('deploy to eks'){
            steps {
                sh 'kubectl get nodes'
                
                sh 'kubectl get all'
                
                sh 'kubectl apply -f webapp.yaml'
                
                sh 'kubectl get pods'
                
                sh 'kubectl get all'
            }
        }
        
    }
}
