pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                 git branch: 'main',
                    url: 'https://github.com/saeedaly/DevOps-Flask-CICD.git',
                    credentialsId: 'f2fc180f-6caf-4a4a-a9a3-fcb34f1dda9f'
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

