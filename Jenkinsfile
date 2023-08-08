pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps {
                echo "Cloning the code"
				git url: "https://github.com/mklinuxdevops/mk-django-notes-app.git", branch: "main"
            }
        }
		stage("Build"){
            steps {
                echo "Building the image"
				sh "docker build -t cicd ."
            }
        }
		stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
				withCredentials([usernamePassword(credentialsId:"DokerHubCredential",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
				sh "docker tag cicd ${env.dockerHubUser}/cicd:latest"
				sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
				sh "docker push ${env.dockerHubUser}/cicd:latest"
				}
            }
        }
		stage("Deploy"){
            steps {
                echo "Deploying the container"
				sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
