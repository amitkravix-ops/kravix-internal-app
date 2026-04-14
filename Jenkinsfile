pipeline {
    agent any

    environment {
        DEV_IP = "100.55.51.240"
        QA_IP = "18.206.236.68"
        PROD_IP = "54.160.196.4"
    }

    stages {

        stage('Pull Code') {
            steps {
                git 'https://github.com/amitkravix-ops/kravix-internal-app'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp .'
            }
        }

        stage('Deploy to Dev') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@$DEV_IP "
                    docker stop myapp || true &&
                    docker rm myapp || true &&
                    docker run -d -p 3001:3000 --name myapp myapp
                    "
                    '''
                }
            }
        }

        stage('Deploy to QA') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@$QA_IP "
                    docker stop myapp || true &&
                    docker rm myapp || true &&
                    docker run -d -p 3002:3000 --name myapp myapp
                    "
                    '''
                }
            }
        }

        stage('Deploy to Prod') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@$PROD_IP "
                    docker stop myapp || true &&
                    docker rm myapp || true &&
                    docker run -d -p 80:3000 --name myapp myapp
                    "
                    '''
                }
            }
        }
    }
}
