pipeline {
    agent {
        docker {
            image 'parthitk/task:dotnet_build'
        }
    }
    environment {
        DOTNET_CLI_HOME = '/tmp'
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage ('Build') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet publish -c Release -o /app/publish'
                echo "Successfully built the code"
            }
        }
    }
}
