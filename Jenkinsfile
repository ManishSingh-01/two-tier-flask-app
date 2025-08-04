pipeline{
    agent any;
    stages{
        stage("Code Clone"){
            steps{
               script{
                   git branch: 'master', url: 'https://github.com/ManishSingh-01/two-tier-flask-app.git'
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
                echo "Developer / Tester tests likh ke dega..."
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerHubCreds","two-tier-flask-app")
                }  
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down"
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
