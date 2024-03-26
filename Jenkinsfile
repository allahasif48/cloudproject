pipeline 
{
    agent any 
    environment {
        // Define environment variables for Docker and Kubernetes
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_IMAGE = 'ecommerce'
        
    }
    options {
        timeout(time:8, unit: 'MINUTES')
            }  //
   
    stages {
        stage('SCM Checkout') 
        {
            steps{
                echo "Checkout..."
                      git url: 'https://github.com/allahasif48/cloudproject.git', 
                          branch: 'master'
                    
            } 
        }

        stage('Check Docker Availability') {
                steps {
                    // Check if Docker CLI is installed
                    script{
                    def dockerInstalled = sh(script: 'command -v docker', returnStatus: true) == 0

                    if (!dockerInstalled) {
                        error('Docker is not installed. Please install Docker on the Jenkins agent.')
                    }

                    // Check Docker version
                    def dockerVersion = sh(script: 'docker --version', returnStdout: true).trim()
                    echo "Docker version: ${dockerVersion}"
                }
            }
        }
        stage('Run Docker Compose') {
            steps {
                script {
                    // Make sure Docker Compose is installed
                    def dockerComposeInstalled = sh(script: 'command -v docker-compose', returnStatus: true) == 0

                    if (!dockerComposeInstalled) {
                        error('Docker Compose is not installed. Please install Docker Compose on the Jenkins agent.')
                    }

                    // Run Docker Compose
                    sh 'docker-compose build'
                    sh 'docker-compose up -d'
                }
            }
        }
stage('Push to Docker Registry') {
            steps {
                // Push Docker image to Docker registry
                script {
                    docker.withRegistry('https://${DOCKER_REGISTRY}', 'docker-hub-credentials') {
                        docker.image("${DOCKER_REGISTRY}/${DOCKER_IMAGE}").push("latest")
                    }
                }
            }
        }
    }   
    post {
        always {
              // Cleanup or additional steps
            sh 'docker-compose down'
       }
    }
}
