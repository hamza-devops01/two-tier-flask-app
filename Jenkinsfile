pipeline{
    
    agent { label "dev" };
        
    stages{
        stage("Code"){
            steps{
                git url:"https://github.com/hamza-devops01/two-tier-flask-app.git", branch: "jenkins"
                echo "Code Clone Successfully"
            }
        }
        
        stage("File System Scan"){
            steps{
                sh "trivy fs . -o result.json"
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
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubcrd",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                 sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
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
            emailext (
                from: 'hamzasajjad3141@gmail.com',
                to: 'hamzasajjad3141@gmail.com',
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build Successful!\n\nConsole log attached hai.",
                attachmentsPattern: '**/result.json',
                mimeType: 'text/plain'
            )
        }
    }
    
    failure {
        script {
            emailext(
                from: 'hamzasajjad3141@gmail.com',
                to: 'hamzasajjad3141@gmail.com',
                subject: "❌ FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build Failed!\n\nConsole log attached hai.",
                attachmentsPattern: '**/result.json',
                mimeType: 'text/plain'
            )
        }
    }
}
}
