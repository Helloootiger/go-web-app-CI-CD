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

        stage("Docker image Build") {
            steps {
                sh ' docker system prune -f'
                sh ' docker container prune -f'
                script {
                    def imageTag = "${docker_hub_username}/go-web-app-new-image:${env.BUILD_NUMBER}"
                    echo "Image tag: ${imageTag}"
                }
            }
        }
    }
}
