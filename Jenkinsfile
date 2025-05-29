pipeline {
    agent any

    environment {
        docker_hub_username = "yfhates"
        GO_PATH = "/usr/local/go/bin"
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
                    export PATH=$PATH:$GO_PATH
                    go build -o go-web-app
                '''
            }
        }

        stage("Test") {
            steps {
                sh '''
                    export PATH=$PATH:$GO_PATH
                    go test ./...
                '''
            }
        }

        stage("Docker Cleanup") {
            steps {
                sh 'docker system prune -f'
                sh 'docker container prune -f'
            }
        }

        stage("Docker Build and Push") {
            steps {
                script {
                    // Compose dynamic image tag using env variables
                    def imageTag = "${env.docker_hub_username}/go-web-app-new-image:${env.BUILD_NUMBER}"
                    echo "Docker Image Tag: ${imageTag}"

                    // Build Docker image
                    sh "docker build -t ${imageTag} ."

                    // Login and push with credentials
                    withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                        sh """
                            echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
                            docker push ${imageTag}
                        """
                    }
                }
            }
        }
    }
}
