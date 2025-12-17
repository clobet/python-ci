pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''
                    . venv/bin/activate
                    sonar-scanner
                    '''
                }
            }
        }

        stage('Deploy (Mock)') {
            steps {
                sh '''
                echo "Starting deployment..."
                echo "Deploying commit: $GIT_COMMIT"
                echo "Deploying to demo environment"
                echo "Deployment successful"
                '''
            }
        }
    }
}
