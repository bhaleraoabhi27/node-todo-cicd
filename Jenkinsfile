pipeline {
    agent { label 'dev-agent' }
    stages {
        stage('code') {
            steps {
                git url: 'https://github.com/bhaleraoabhi27/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('build and test') {
            steps {
                sh 'docker build . -t abhibhalerao/node-todo-app-cicd:latest'
            }
        }
        stage('login and push image') {
            steps {
                echo "login into docker hub and pushing image"
                withCredentials([usernamePassword(credentialsId:'dockerHub',passwordVariable:'dockerHubPass',usernameVariable:'dockerHubUser')]){
                    sh idocker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}'  
                    sh 'docker push abhibhalerao/node-todo-app-cicd:latest'
                }
            }
        }
        stage('deploy') {
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
