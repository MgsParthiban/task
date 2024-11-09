pipeline {
    agent {
        docker {
            image 'parthitk/task:image2'
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
                sh 'dotnet publish -c Release -o out'
                echo "Successfully built the code"
            }
        }
        stage('Static Code Analysis') {
            environment {
                SONAR_URL = "http://65.0.180.122:9000/"
            }
            steps {
                withCredentials([string(credentialsId: 'sonar', variable: 'SONAR_AUTH_TOKEN')]) {
                    sh 'dotnet sonarscanner begin /k:"myapp" /d:sonar.host.url=$SONAR_URL /d:sonar.login=$SONAR_AUTH_TOKEN'
                    sh 'dotnet build'
                    sh 'dotnet sonarscanner end /d:sonar.login=$SONAR_AUTH_TOKEN'
                }
            }
        }
    }
}
