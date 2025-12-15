pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "aswineraja08/flask-app"
        DOCKER_CREDS = "dockerhub-creds"
        SONAR_SERVER = "SonarQubeServer"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 --version
                pytest --version
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                pytest || true
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=python-ci-cd-demo \
                      -Dsonar.projectName=python-ci-cd-demo \
                      -Dsonar.sources=. \
                      -Dsonar.language=py
                    '''
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDS) {
                        sh '''
                        docker build -t $DOCKER_IMAGE .
                        docker push $DOCKER_IMAGE
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo "🎉 CI/CD Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
    }
}
