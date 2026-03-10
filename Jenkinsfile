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
-Dsonar.host.url=http://sonarqube:9000
-Dsonar.login=squ_9b31746c1f52a882a80e8c1b55cf672da471459a
        '''
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
