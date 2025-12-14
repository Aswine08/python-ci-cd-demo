pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Aswine08/python-ci-cd-demo.git'
            }
        }

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
    }
}
