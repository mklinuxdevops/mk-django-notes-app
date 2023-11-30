pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                echo 'Cloning Code'
                git url: "https://github.com/mklinuxdevops/mk-django-notes-app.git", branch: "main"
            }
		}	
		stage('Build') {
            steps {
                echo 'Build code'
                sh "docker build -t notes-app ." 
            }
		}	
		stage('Push to Docker Hub') {
            steps {
                echo 'Pushing imange to Docker Hub'
                withCredentials([usernamePassword(credentialsId:"dockerHubPass",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes-app ${env.dockerHubUser}/notes-app-new:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app-new:latest"
                            }
            }            
		}	
		stage('Deploy') {
            steps {
                echo 'Deploy the contener'
                sh "docker-compose down && docker-compose up -d"
            }	
		}	
       
    }
}
