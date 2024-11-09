pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('docker build') {
            steps {
                sh 'docker build -t image1 .'
            }
        }
    }
}    
