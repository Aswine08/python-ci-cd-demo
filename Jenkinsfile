pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh '''
                pip3 install -r requirements.txt --break-system-packages
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
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('SonarQubeServer') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                          -Dsonar.projectKey=python-ci-cd-demo \
                          -Dsonar.projectName=python-ci-cd-demo \
                          -Dsonar.sources=. \
                          -Dsonar.language=py
                        """
                    }
                }
            }
        }
    }
}
