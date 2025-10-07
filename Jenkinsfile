pipeline{
    agent {label "agent vinod"}
    stages{
        stage("Cloning"){
            steps{
                echo "Code cloning has started"
                git url : "https://github.com/wadood786/Notes-app.git" , branch: "main"
                echo "Code has cloned successfully"
            }
        }
        stage("Build"){
            steps{
                echo "Build has started"
                sh "docker build -t notes-app:latest ."
                echo "Image has created successfully"
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "Image is pushing to dockerhub"
                withCredentials([usernamePassword('credentialsId':"dockerhubcred",
                passwordVariable:"dockerHubPass",
                usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "now the app is ready to deploy"
                sh "docker compose up -d"
                echo "App has deployed successfully"
            }
        }
    }
}
