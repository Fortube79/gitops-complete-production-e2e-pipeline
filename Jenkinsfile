pipeline{
    agent{
        label "agent-01"
    }
    environment{
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }

    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }

        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github-pat', url: 'https://github.com/Fortube79/gitops-complete-production-e2e-pipeline'
            }
        }

        stage("Update the Deployment Tags"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """    
            }
        }

        stage("Push the changed deployment file to Git"){
            steps{
                sh """
                    git config --global user.name "Fortube79"
                    git config --global user.email "f.ortube@gmail.com"
                    git add deployment.yaml
                    git commit -m "Update Deployment Manifest"
                   """
                withCredentials([gitUsernamePassword(credentialsId: 'github-pat', gitToolName: 'default')]){
                    sh "git push https://github.com/Fortube79/gitops-complete-production-e2e-pipeline main"
                }     
            }
        }    
    }
}
