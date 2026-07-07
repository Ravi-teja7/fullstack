pipeline {
    agent any

    environment {
        APP_DIR = "/home/ec2-user/fullstack/fullstack"
    }

    stages {

        stage('Update Project') {
            steps {
                sh '''
                cd $APP_DIR
                git pull origin main
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                cd $APP_DIR/frontend
                npm install
                npm run build
                '''
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                sudo rm -rf /usr/share/nginx/html/*
                sudo cp -r $APP_DIR/frontend/dist/* /usr/share/nginx/html/
                '''
            }
        }

        stage('Setup Backend') {
            steps {
                sh '''
                cd $APP_DIR/backend
                python3 -m venv venv || true
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Restart FastAPI') {
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
            echo '======================================'
            echo 'Deployment completed successfully!'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo 'Deployment failed!'
            echo 'Check the Jenkins console output.'
            echo '======================================'
        }
    }
}