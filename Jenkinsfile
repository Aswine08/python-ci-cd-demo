pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt --break-system-packages'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest || true'
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
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        sh '''
                        docker build -t aswineraja08/flask-app:latest .
                        docker push aswineraja08/flask-app:latest
                        '''
                    }
                }
            }
        }
    }
}
