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
    } 
   post {
        always {
              // Cleanup or additional steps
            sh 'docker-compose down'
       }
    }
  
}


/*
        stage('Run Docker Compose File')
    {
        sh 'sudo docker-compose build'
        sh ' sudo docker-compose up -d'
    }
  
  stage('PUSH image to Docker Hub')
    {
       withCredentials([string(credentialsId: 'DockerHubPassword', variable: 'DHPWD')]) 
        {
            sh "docker login -u upasanatestdocker -p ${DHPWD}"
        }
        sh 'docker push vardhanns/phpmysql_app'
        
        docker.withRegistry( 'https://registry.hub.docker.com', 'DockerHubPassword' ) 
             
             sh 'sudo docker login -u "upasanatestdocker" -p "Zephyr@17" docker.io'
             sh 'sudo docker push upasanatestdocker/mysql'
             sh 'sudo docker push upasanatestdocker/job1_web1.0'
             sh 'sudo docker push upasanatestdocker/job1_web2.0'
             sh 'docker push upasanatestdocker/mysql'
          
    }
*/

