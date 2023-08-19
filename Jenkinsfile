pipeline {
    agent { label 'dev-agent' }
    stages {
        stage('Code') {
            steps {
                script {
                    properties([pipelineTriggers([pollSCM('')])])
                }
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
                echo "docker hub login and push image"
                withCredentails([usernamePassword(credentailsID:'docker-hub',passwordVariables:'dockerHubPassword',usernameVariables:'dockerHubUsername')]) {
                    sh "docker login -u ${env.dockerHubUsername} -p ${env.dockerHubPassword}"
                    sh "docker push abhibhalerao/node-todo-app-cicd:latest"
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
