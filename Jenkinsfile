pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/gauravmishra52/gitops-register-app'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

       stage("Push the changed deployment file to Git") {
    steps {
        withCredentials([usernamePassword(credentialsId: 'github-push-token', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASSWORD')]) {
            sh '''
                git config --global user.name "gauravmishra52"
                git config --global user.email "gaurav.mishra.cs.2022@mitmeerut.ac.in"
                git add deployment.yaml
                git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                git push https://$GIT_USER:$GIT_PASSWORD@github.com/gauravmishra52/gitops-register-app.git main
            '''
        }
    }
}
    }   
}
