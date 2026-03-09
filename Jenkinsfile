pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        SONAR_SCANNER = 'C:/Users/ELECTRO-HM/Downloads/sonar-scanner-cli-8.0.1.6346-windows-x64/sonar-scanner.bat'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = 'squ_3735528ba19050ccf0c6eef13caa00fda2f8d8f0'
        SONAR_PROJECT_KEY = 'TP-Jenkins-Security'
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
                python -m pip install --upgrade pip
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
                "${SONAR_SCANNER}" ^
                  -Dsonar.projectKey=%SONAR_PROJECT_KEY% ^
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
                python -m pip install --upgrade pip
                python -m pip install pip-audit
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
