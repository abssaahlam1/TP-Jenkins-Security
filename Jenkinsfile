pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = 'squ_3735528ba19050ccf0c6eef13caa00fda2f8d8f0'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/abssaahlam1/TP-Jenkins-Security.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat """
                python -m venv %VENV_DIR%
                call %VENV_DIR%\\Scripts\\activate.bat
                pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                bat """
                call %VENV_DIR%\\Scripts\\activate.bat
                pytest
                """
            }
        }

        stage('SAST Scan') {
            steps {
                bat """
                sonar-scanner ^
                  -Dsonar.projectKey=TP-Jenkins-Security ^
                  -Dsonar.sources=. ^
                  -Dsonar.host.url=%SONAR_HOST_URL% ^
                  -Dsonar.login=%SONAR_TOKEN%
                """
            }
        }

        stage('SCA Scan') {
            steps {
                bat """
                call %VENV_DIR%\\Scripts\\activate.bat
                pip install --upgrade pip
                pip install pip-audit
                pip-audit
                """
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
