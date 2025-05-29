pipeline {
    agent any

    tools {
        go 'go'  // Make sure you have a Go installation named 'go' in Jenkins tools config
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
    }
}