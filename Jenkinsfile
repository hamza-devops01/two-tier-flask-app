@Library("Shared") _
pipeline{
    
    agent { label "dev" };
        
    stages{
        stage("Code"){
            steps{
                script{
                clone("https://github.com/hamza-devops01/two-tier-flask-app.git", "jenkins")
                echo "Code Clone Successfully"
            }
            }     
        }
        
        stage("File System Scan"){
            steps{
                script{
                   trivy-fs()
                }
            }
        }
        
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        
        stage("Test"){
            steps{
                echo "Developer/Tester test the code"
            }
        }
        
        stage("push to Docker Hub"){
            steps{
                script{
                    docker-push("dockerhubcrd","two-tier-flask-app")
                } 
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    
post {
    success {
        script {
           email-notify(env.JOB_NAME, env.BUILD_NUMBER, true)
        }
    
}
    failure {
        script {
           email-notify(env.JOB_NAME, env.BUILD_NUMBER, false)
        }
    
}
}
}
