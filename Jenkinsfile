pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out by Jenkins'
            }
        }

        stage('Test') {
            steps {
                sh 'python3 -m pytest -q'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-student-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f devops-app || true

                    docker run -d \
                      --name devops-app \
                      -p 80:5000 \
                      devops-student-app:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'sleep 5'
                sh 'curl -f http://localhost/health'
            }
        }
    }

    post {
        success {
            echo 'APPLICATION DEPLOYED SUCCESSFULLY!'
        }

        failure {
            echo 'PIPELINE FAILED - CHECK THE CONSOLE OUTPUT'
        }
    }
}
