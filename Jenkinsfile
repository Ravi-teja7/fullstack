pipeline {
    agent any

    environment {
        PROJECT_DIR = "/home/ec2-user/fullstack/fullstack"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh 'sudo cp -r frontend/dist/* /usr/share/nginx/html/'
            }
        }

        stage('Setup Backend') {
            steps {
                dir('backend') {
                    sh '''
                    python3 -m venv venv || true
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Restart Backend') {
            steps {
                sh 'sudo systemctl restart fastapi'
            }
        }

        stage('Restart Nginx') {
            steps {
                sh 'sudo systemctl restart nginx'
            }
        }

    }

    post {
        success {
            echo 'Application deployed successfully!'
        }

        failure {
            echo 'Deployment failed.'
        }
    }
}