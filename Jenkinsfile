pipeline {
    agent any;

    stages {
        stage("code") {
            steps {
                git url: "https://github.com/YashaswiKumars/two-tier-flask-app.git", branch: "master"
            }
        }
        
        stage("build") {
            steps {
               sh "docker build -t two-tier-flask-app ."
            }
        }
        
        stage("Test") {
            steps {
                echo "test bhi done"
            }
        }
        stage("Push to Docker HUb"){
            steps{
               withCredentials([usernamePassword(
                   credentialsId:"DockerHubCreds",
                   passwordVariable: "DockerHubPass",
                   usernameVariable: "DockerHubUser"
                   )]){
                       sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass} "
                       sh "docker image tag two-tier-flask-app ${env.DockerHubUser}/two-tier-flask-app " 
                       sh "docker push ${env.DockerHubUser}/two-tier-flask-app:latest "
                   }
               
            }
        }
        
        stage("deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
