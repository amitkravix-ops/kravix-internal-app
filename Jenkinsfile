pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh '/usr/local/bin/docker build -t myjenkinsapp .'
            }
        }
        stage('Run Container') {
            steps {
                sh '/usr/local/bin/docker run -d -p 8095:80 --name jenkinscontainer myjenkinsapp || true'
            }
        }
    }
}
