pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t docker-jenkins-demo:${BUILD_NUMBER} .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker rm -f docker-jenkins-demo || true
                    docker run -d --name docker-jenkins-demo -p 8081:80 docker-jenkins-demo:${BUILD_NUMBER}
                '''
            }
        }

        stage('Test Application') {
            steps {
                sh 'curl --fail http://localhost:8081'
            }
        }
    }

    post {
        always {
            sh 'docker ps -a'
        }
    }
}
