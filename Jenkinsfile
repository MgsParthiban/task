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
                sh 'ls -lrt'
              sh 'cd d2k && dontnet sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
            }
          }
    }
    }
}
