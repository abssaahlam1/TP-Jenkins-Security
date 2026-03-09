pipeline {
    agent any

    environment {
        VENV_DIR = 'venv' // Virtual environment folder
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
                if [ ! -d "$VENV_DIR" ]; then
                    python3 -m venv $VENV_DIR
                fi
                . $VENV_DIR/bin/activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                pip install sonar-scanner
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                pytest
                '''
            }
        }

        stage('SAST Scan') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                sonar-scanner \
                    -Dsonar.projectKey=TP-Jenkins-Security \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://<SONARQUBE_SERVER>:9000 \
                    -Dsonar.login=<SONARQUBE_TOKEN>
                '''
            }
        }

        stage('SCA Scan') {
            steps {
                sh '''
                /opt/dependency-check/dependency-check.sh \
                    --project "TP-Jenkins" \
                    --scan . \
                    --format HTML \
                    --out reports/ \
                    --failOnCVSS 7
                '''
            }
        }

    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed due to errors or vulnerabilities'
        }
    }
}
