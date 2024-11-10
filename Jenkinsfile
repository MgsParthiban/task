pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "parthitk/task:${BUILD_NUMBER}"
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub')
        AWS_CREDENTIALS = credentials('aws-credentials-id')
        EC2_CREDENTIALS = credentials('aws_ssh')
    }

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['UAT', 'Production'], description: 'Choose the environment to deploy to')
    }

    stages {
        stage('Clone Code') {
            steps {
                echo "scm checkout"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "docker-hub") {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Set the EC2 IP address based on the selected environment
                    def ec2Ip = (params.ENVIRONMENT == 'UAT') ? "${env.UAT-EC2-IP}" : env.Production-EC2-IP
                    def imageTag = "parthitk/task:${BUILD_NUMBER}"

                    // Use sshagent to manage the SSH key from Jenkins credentials
                    sshagent(credentials: ['aws_ssh']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${ec2Ip} "
                            docker pull ${imageTag} &&
                            docker run -d -p 5000:5000 ${imageTag}
                        "
                        """
                    }
                }
            }
        }

       /*  stage('Health Check') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'UAT') {
                        sh 'curl -f http://UAT-EC2-IP:5000/ || exit 1'
                    } else {
                        sh 'curl -f http://Production-EC2-IP:5000/ || exit 1'
                    }
                }
            }
        }*/
    }
}

