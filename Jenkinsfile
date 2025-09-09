pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-app"
        DOCKER_REGISTRY = ""   // Empty for local demo
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<username>/demo-ci-cd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Deploy via Ansible') {
            steps {
                ansiblePlaybook(
                    playbook: 'ansible/deploy.yml'
                )
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
