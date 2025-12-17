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
                bat '''
                python -m venv venv
                venv\\Scripts\\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                venv\\Scripts\\activate
                pytest
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    bat '''
                    venv\\Scripts\\activate
                    sonar-scanner
                    '''
                }
            }
        }

        stage('Deploy (Mock)') {
            steps {
                bat '''
                echo Starting deployment...
                echo Deploying commit %GIT_COMMIT%
                echo Deploying to demo environment
                echo Deployment successful
                '''
            }
        }
    }
}
