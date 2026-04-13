pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'feature-login', url: 'https://github.com/amitkravix-ops/kravix-internal-app'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp .'
            }
        }
        stage('Stop Old Container') {
            steps {
                sh 'docker stop mycontainer || true'
                sh 'docker rm mycontainer || true'
            }
        }
        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name mycontainer myapp'
            }
        }
    }
}
