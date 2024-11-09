pipeline {
    agent {
        docker {
            image 'parthitk/task:image3'  // Use your Docker image
        }
    }
    environment {
        DOTNET_CLI_HOME = '/tmp'
        PATH = "/root/.dotnet/tools:${env.PATH}"  // Ensure SonarScanner is in PATH
        SONAR_URL = "http://65.0.180.122:9000/"  // SonarQube server URL
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm  // Get the code from the repository
            }
        }
        stage('Build and Analyze') {
            steps {
                withCredentials([string(credentialsId: 'sonar', variable: 'SONAR_AUTH_TOKEN')]) {
                    // Step 1: Begin SonarQube analysis
                    sh 'dotnet sonarscanner begin /k:"myapp" /d:sonar.host.url=$SONAR_URL /d:sonar.login=$SONAR_AUTH_TOKEN'

                    // Step 2: Build the .NET project
                    sh 'dotnet build'

                    // Step 3: End SonarQube analysis and submit results
                    sh 'dotnet sonarscanner end /d:sonar.login=$SONAR_AUTH_TOKEN'
                }
            }
        }
    }
}
