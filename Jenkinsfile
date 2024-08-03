pipeline {
    
    agent { 
        node{
            label "dev"
            
        }
    }
    
    stages{
        stage("Clone Code"){
            steps{
                echo " git clone step is running"
                git url: "https://github.com/umeashpatl07/django-notes-app.git", branch: "main"
                echo "WEBHOOK IMPLEMEMT KIA HAI...
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t notes-app-jenkins:latest"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockercreds",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker-compose up -d"
            }
        }
    }
}
