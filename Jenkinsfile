pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/abssaahlam1/TP-Jenkins-Security.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest
                '''
            }
        }

        stage('SAST Scan') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    . venv/bin/activate
                    sonar-scanner \
                        -Dsonar.projectKey=tp-jenkins-security \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://sonarqube:9000 \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('SCA Scan') {
            steps {
                sh '''
                dependency-check.sh \
                    --project "TP-Jenkins" \
                    --scan . \
                    --format HTML \
                    --failOnCVSS 7
                '''
            }
        }
    }

    post {
        failure {
            echo 'Build failed'
        }
    }
}
