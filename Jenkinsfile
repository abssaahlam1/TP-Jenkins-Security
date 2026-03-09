pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/abssaahlam1/TP-Jenkins-Security.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv ${VENV_DIR}
                . ${VENV_DIR}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . ${VENV_DIR}/bin/activate
                pytest
                '''
            }
        }

        stage('SAST Scan') {
            steps {
                withSonarQubeEnv('MySonarQubeServer') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('SCA Scan') {
            steps {
                sh '''
                . ${VENV_DIR}/bin/activate
                pip install --upgrade pip
                pip install pip-audit
                pip-audit
                '''
            }
        }

    }

    post {
        failure {
            echo '❌ Build failed'
        }
        success {
            echo '✅ Build succeeded'
        }
    }
}
