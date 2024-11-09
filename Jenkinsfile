pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage ('docker build') {
            steps {
                docker build -t image1 .
            }
        }

    }
}    
