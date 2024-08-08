pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/sri2778/two-tier-app.git", branch: "main"
            }
        }
        stage("Build Image"){
            steps{
                sh "docker build -t messageapp ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker tag messageapp ${env.dockerhubuser}/messageapp:latest"
                    sh "docker push ${env.dockerhubuser}/messageapp:latest" 
                }
            }
        }
        stage("Deploy application"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
