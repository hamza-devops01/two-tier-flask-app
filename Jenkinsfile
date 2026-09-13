pipeline{
    agent { label "dev" };
    stages{
        stage("Code"){
            steps{
                git url:"https://github.com/hamza-devops01/two-tier-flask-app.git", branch: "jenkins"
                echo "Code Clone Successfully"
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
    
    post{
        success{
            mail to: 'hamzasajjad3141@gmail.com',
            subject: 'Success:Job Completed',
            body: 'Your Pipeline run Successfully!'
        }
        failure{
            mail to: 'hamzasajjad3141@gmail.com',
            subject: 'Failure:Build Failed',
            body: 'Check the Jenkins console output for errors.'
        }
    }
}
