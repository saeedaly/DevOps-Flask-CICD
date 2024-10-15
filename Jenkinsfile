pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/DevOps-Flask-CICD.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('flask-app')
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

