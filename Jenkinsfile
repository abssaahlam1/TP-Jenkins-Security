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
        sh '''
        . venv/bin/activate
        sonar-scanner \
        -Dsonar.projectKey=tp-jenkins-security \
        -Dsonar.sources=. \
        -Dsonar.host.url=http://localhost:9000 \
        -Dsonar.login=YOUR_TOKEN
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
