pipeline 
{
    agent any
    options {
        timeout(time:8, unit: 'MINUTES')
            }
   
    stages {
        stage('SCM Checkout') 
        {
            steps{
                echo "Checkout..."
                      git url: 'https://github.com/VarhaKhan/cloudproject.git', 
                          branch: 'feature'
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
      stage('Push Docker Images to Docker Hub') {
    steps {
        script {
            // Tagging and pushing the first image
            sh 'docker tag webjob-web:latest varha/myonlineapp-webjob-web:newimagev1'
            
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub_id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
                sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
            }
            sh 'docker push varha/myonlineapp-webjob-web:newimagev1'

            // Tagging and pushing the second image
            sh 'docker tag mysql:latest varha/myonlineapp-mysql:newimagev2'
            sh 'docker push varha/myonlineapp-mysql:newimagev2'
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
}




    




        
