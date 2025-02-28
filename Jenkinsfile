pipeline{
    
    agent {label "dev"};
    stages{
        stage("CODE"){
            steps{
                git url: "https://github.com/jayantsonone/two-tier-flask-app.git", branch: "master"
            }
        }
        
        stage("BUILD"){
            steps{
                sh "docker build -t my-app ."
            }
        }
        
        stage("TEST"){
            steps{
                echo "testing later"
            }
        }
        stage("Push to DockeHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhub",
                    passwordVariable: "dockerhubpass",
                    usernameVariable: "dockerhubuser"
                    )]){
                        
                        sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                        sh "docker image tag my-app ${env.dockerhubuser}/my-app"
                        sh "docker push ${env.dockerhubuser}/my-app:latest"
                    
                }
            }
        }
        stage("DEPLOY"){
            steps{
                sh "docker compose up -d --build"
            }
        }
    }
    post{
        success{
            script{
                emailext from: 'jsonone0@gmail.com'
                to: 'jsonone0@gmail.com'
                subject: 'build success'
                body: 'build is successfull'
            }
        }
        failure{
            script{
                emailext from: 'jsonone0@gmail.com'
                to: 'jsonone0@gmail.com'
                subject: 'build failure'
                body: "build failed"
            }
        }
    }
}
