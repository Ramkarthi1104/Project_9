pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')  // we'll add this next
        DOCKER_IMAGE = "yourdockerhubusername/myapp:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-pat',
                    url: 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'dockerhub-creds', url: 'https://index.docker.io/v1/']) {
                        sh "docker push $DOCKER_IMAGE"
                    }
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i inventory deploy.yml'
            }
        }
    }
}
