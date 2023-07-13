pipeline{
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "Cloning the code again"
                git url:"https://github.com/chavansaurabh216/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps{
                echo "Building the image"
                sh "docker build -t noteapp ."
            }
        }
        stage("push to docker hub"){
            steps{
                 echo "Pushing to Docker Hub"
                 withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerhubUser')]) {
        	        sh "docker tag noteapp ${env.dockerhubUser}/my-notes-app:latest"
        	        sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
                    sh "docker push ${env.dockerhubUser}/my-notes-app:latest"
                 }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying to the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
