pipeline {
    agent any

    environment {
        IMAGE_NAME = "amitkumarbehera/myapp"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/amitkravix-ops/kravix-internal-app.git'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                ssh ec2-user@13.60.218.186 << EOF
                docker pull $IMAGE_NAME:latest
                docker stop myapp || true
                docker rm myapp || true
                docker run -d -p 80:3000 --name myapp $IMAGE_NAME:latest
                EOF
                '''
            }
        }
    }
}
