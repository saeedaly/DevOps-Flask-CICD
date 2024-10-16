pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASSWORD')]) {
                def giturl: 'https://$GIT_USER:$GIT_PASSWORD@github.com/saeedaly/DevOps-Flask-CICD.git'
                    git url: gitUrl
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh 'docker login -u $USERNAME -p $PASSWORD'
                        docker.build('flask-app')
                    }
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    docker.image('flask-app').inside {
                        sh 'pytest tests/test_app.py'
                    }
                }
                                
            }
        }
        stage('Deploy to Server') {
            steps {
                ansiblePlaybook credentialsId: 'ansible-key', inventory: 'ansible/inventory/hosts', playbook: 'ansible/playbooks/deploy.yml'
            }
        }
    }
}
