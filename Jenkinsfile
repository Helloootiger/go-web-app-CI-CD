pipeline {
    agent any

     environment{
        docker_hub_username= "yfhates"   
     }

    stages {
        stage("Cleaning Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Git Checkout") {
            steps {
                git credentialsId: 'git-cred', url: 'https://github.com/AmeyD090/go-web-app-devops.git', branch: 'main'
            }
        }

        stage("Build") {
            steps {
                sh '''
                    export PATH=$PATH:/usr/local/go/bin
                    go build -o go-web-app
                    '''
            }
        }

        stage("Test") {
            steps {
                sh '''
                  export PATH=$PATH:/usr/local/go/bin
                  go test ./...

                  '''
            }
        }

        stage("Docker image Build"){
            steps{
                sh 'sudo docker system prune -f'
                sh 'sudo docker container prune -f'
                // Define the image tag using Jenkins build number
                  def imageTag = "${docker_hub_username}/go-web-app-new-image:${BUILD_NUMBER}" //check the image name
            }
        }
    }
}