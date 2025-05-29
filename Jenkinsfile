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
                    def imageTag = "${env.docker_hub_username}/go-web-app:${env.BUILD_NUMBER}"
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

        stage("Update tag in helm chart"){
            
                environment{
                    GIT_REPO_NAME = "https://github.com/AmeyD090/go-web-app-devops.git"
                     GIT_USER_NAME = "AmeyD090"
                }

                steps{
                    dir("helm/go-web-app-chart"){
                        withcredentials([usernamePassword(
                            credentialsId:'github-cred',
                        usernameVariable:'GITHUB_USER',
                        passwordVariable:'GITHUB_PASS')]){
                            sh '''
                            git config user.email "ameydeshmukh090@gmail.com" ,
                            git config user.name "AmeyD090"
                            
                            # Get the build number
                            BUILD_NUMBER=${BUILD_NUMBER}
                            echo "Current BUILD_NUMBER: $BUILD_NUMBER"

                            # Extract the current tag from values.yaml
                             imageTag=$(grep -oP "(?<=tag:\\s)[^\\n]+" values.yaml)
                             echo "Current image tag: $imageTag"

                             # Replace the tag value with the build number
                             sed -i "s/tag: ${imageTag}/tag: ${BUILD_NUMBER}/" values.yaml


                            # Commit and push the changes
                             git add values.yaml
                             git commit -m "Update Image tag in values.yaml to version ${BUILD_NUMBER}"
                            git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/go-web-app-devops.git HEAD:main

                            '''


                        }
                    }
                }

            }
        
    }
}
