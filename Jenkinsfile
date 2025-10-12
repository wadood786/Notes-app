@Library("Shared") _
pipeline {
    agent any

    stages {

        stage("Cloning") {
            steps {
                script {
                    cloning("https://github.com/wadood786/Notes-app.git", "main")
                }
            }
        }
        stage("trivy File system scanning"){
            steps{
                script{
                    trivy_scan()
                }
            }
            
        }

     

        // 🧰 Step 5: Docker build and push
        stage("Build") {
            steps {
                echo "Building Docker image..."
                script {
                    docker_building("notes-app", "latest", "abdulwadood786")
                }
                echo "Image built successfully!"
            }
        }

        stage("Push to DockerHub") {
            steps {
                echo "Pushing Docker image to DockerHub..."
                script {
                    docker_pushing("notes-app", "latest", "abdulwadood786")
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying app..."
                sh "docker compose up -d"
                echo "App deployed successfully!"
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Pipeline completed successfully!"
            archiveArtifacts artifacts: '*.xml', followSymlinks: false
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
    }
}
