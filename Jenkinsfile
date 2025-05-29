pipeline {
    agent any

    environment {
        docker_hub_username = "yfhates"
    }

    stages {
        stage("Cleaning Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Git Checkout") {
            steps {
                git credentialsId: 'github-cred', url: 'https://github.com/AmeyD090/go-web-app-devops.git', branch: 'main'
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

        stage("Docker image Build adn Tag") {
            steps {
                sh ' docker system prune -f'
                sh ' docker container prune -f'
                script {
                    def imageTag = "${docker_hub_username}/go-web-app-new-image:${env.BUILD_NUMBER}"
                    echo "Image tag: ${imageTag}"
                }
            }
        }
        stage("Docker images push to DockerHub "){
            steps{
                sh '''
                echo ${docker_hub_password} | sudo docker login -u ${docker_hub_username} --password-stdin
                docker push ${imageTag}"
                '''
            }
        }
    }
}
