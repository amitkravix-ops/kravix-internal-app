pipeline {
    agent any

    environment {
        IMAGE_NAME = "amitkumarbehera/amit-myapp"
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

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh '''
                ssh ec2-user@$DEV_IP << EOF
                docker pull $IMAGE_NAME:$BUILD_NUMBER
                docker run -d -p 3001:3000 $IMAGE_NAME:$BUILD_NUMBER
                EOF
                '''
            }
        }

        stage('Approval for QA') {
            steps {
                input message: "Deploy to QA?"
            }
        }

        stage('Deploy to QA') {
            steps {
                sh '''
                ssh ec2-user@$QA_IP << EOF
                docker pull $IMAGE_NAME:$BUILD_NUMBER
                docker run -d -p 3002:3000 $IMAGE_NAME:$BUILD_NUMBER
                EOF
                '''
            }
        }

        stage('Approval for Prod') {
            steps {
                input message: "Deploy to Production?"
            }
        }

        stage('Deploy to Prod') {
            steps {
                sh '''
                ssh ec2-user@$PROD_IP << EOF
                docker pull $IMAGE_NAME:$BUILD_NUMBER
                docker run -d -p 80:3000 --restart always $IMAGE_NAME:$BUILD_NUMBER
                EOF
                '''
            }
        }
    }
}
