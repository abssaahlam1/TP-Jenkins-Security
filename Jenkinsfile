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
                    /opt/sonar-scanner/bin/sonar-scanner \
                      -Dsonar.projectKey=tp-jenkins-security \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=http://172.18.0.2:9000 \
                      -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

       stage('SCA Scan') {
    steps {
        sh '''
            docker run --rm -v $PWD:/src owasp/dependency-check:latest \
              --project "TP-Jenkins" \
              --scan /src \
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
