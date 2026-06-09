pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "845844105934.dkr.ecr.ap-south-1.amazonaws.com/register-app"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'GitHubAccess', url: 'https://github.com/NikhilKapade/gitops-register-app'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's|image: .*|image: ${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "NikhilKapade"
                   git config --global user.email "nikhilkapade29@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest" || true
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHubAccess', gitToolName: 'Default')]) {
                  sh "git push origin main"
                }
            }
        }
      
    }
}
