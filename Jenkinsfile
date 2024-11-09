pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stages ('docker build') {
            steps {
                docker build -t image1 .
            }
        }

    }    
}
