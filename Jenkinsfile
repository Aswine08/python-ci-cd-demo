pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh '''
                sudo apt-get update -y
                sudo apt-get install -y python3-pip unzip curl

                pip3 install pytest --break-system-packages
                '''
            }
        }

        stage('Install Sonar Scanner') {
            steps {
                sh '''
                if [ ! -d "/opt/sonar-scanner" ]; then
                  sudo curl -o /tmp/sonar-scanner.zip \
                  https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip

                  sudo unzip /tmp/sonar-scanner.zip -d /opt
                  sudo mv /opt/sonar-scanner-* /opt/sonar-scanner
                fi

                sudo ln -sf /opt/sonar-scanner/bin/sonar-scanner /usr/bin/sonar-scanner
                '''
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
    }
}
