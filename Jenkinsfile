@Library("Shared") _
pipeline {
    agent { label "agent vinod" }

    environment {
        SONAR_HOME = tool "Sonar"   // Must match Jenkins global tool name
    }

    stages {

        stage("Cloning") {
            steps {
                script {
                    cloning("https://github.com/wadood786/Notes-app.git", "main")
                }
            }
        }

        // 🧪 Step 1: Trivy - Filesystem vulnerability scan
        stage("Trivy: Filesystem Scan") {
            steps {
                script {
                    trivy_scan()
                }
            }
        }

        // 🧩 Step 2: OWASP Dependency Vulnerability Scan
        stage("OWASP: Dependency Check") {
            steps {
                script {
                    owasp_dependency()
                }
            }
        }

        // 🧮 Step 3: SonarQube Code Analysis
        stage("SonarQube: Code Analysis") {
            steps {
                script {
                    sonarqube_analysis("Sonar", "notes-app", "notes-app")
                }
            }
        }

        // ✅ Step 4: SonarQube Quality Gate
        stage("SonarQube: Quality Gate") {
            steps {
                script {
                    sonarqube_code_quality()
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
