pipeline {
    agent any

    environment {
        GOROOT = '/usr/local/go'
        PATH = "${GOROOT}/bin:${env.PATH}"
    }

    stages {
        stage("Install Go") {
            steps {
                sh '''
                    wget https://golang.org/dl/go1.22.0.linux-amd64.tar.gz
                    sudo rm -rf /usr/local/go
                    sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
                    go version
                '''
            }
        }

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
                sh 'go build -o go-web-app'
            }
        }

        stage("Test") {
            steps {
                sh 'go test ./...'
            }
        }
    }
}
