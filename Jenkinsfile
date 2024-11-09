pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage ('build') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet publish -c Release -o /app/publish'   
                echo "sucessfully builld the code"         }
        }

    }
}    

