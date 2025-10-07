@Library("Shared") _
pipeline{
    agent {label "agent vinod"}
    stages{
        stage("Cloning"){
            steps{
              script{
                  clone("https://github.com/wadood786/Notes-app.git" , "main")
              }
            }
        }
        stage("Build"){
            steps{
                echo "Build has started"
               script{
                   docker_build("notes-app","latest","abdulwadood61999@gmqil.com")
               }
                echo "Image has created successfully"
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "Image is pushing to dockerhub"
                script{
                    docker_push("notes-app",""latest","abdulwadood61999@gmail.com")
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
