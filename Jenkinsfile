pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code downloaded from GitHub'
            }
        }

        stage('Check Versions') {
            steps {
                sh 'git --version'
                sh 'node -v'
                sh 'npm -v'
                sh 'python3 --version'
            }
        }

    }
}