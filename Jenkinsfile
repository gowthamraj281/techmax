pipeline {
    agent any
    tools {
        maven "maven"
    }
    
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/gowthamraj281/techmax.git']])
                bat "mvn clean install"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t gowthamraj281/devops ."
                }
            }
        }
        stage("Push Image to hub"){
            steps {
                script {
                    withCredentials([string(credentialsId: 'admin2', variable: 'admin2')]) {
                    bat "docker login -u gowthamraj281 -p ${admin2}"
}
                    bat "docker push gowthamraj281/devops"
                    
                }
            }
        }
        
        stage("Deploy to K8's") {
            steps {
                script {
                    kubernetesDeploy (configs:'deploymentservice.yaml', kubeconfigId: 'demokube')
                }
            }
        }
    }
}
