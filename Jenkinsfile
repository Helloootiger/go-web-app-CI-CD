pipeline {
    agent any

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
    }
}