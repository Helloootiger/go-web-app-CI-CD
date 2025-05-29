pipeline{
    agent any

    tools{
        go 'go'
    }

    stages{
        stage("Cleaning Workspace"){
            steps{
                CleanWs()
            }
        }

        stage("Git Checkout"){
            steps{
                git credentialsId:"git-cred" url:"https://github.com/AmeyD090/go-web-app-devops.git" branch:"main"
            }
        }
    }
}