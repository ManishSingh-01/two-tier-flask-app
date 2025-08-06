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
        stage('Code Quality Check') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
                }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app:latest ."
            }
            
        }
        stage("Test"){
            steps{
                echo "Developer / Tester tests likh ke dega..."
            }
            
        }
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub_cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh """
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker tag two-tier-flask-app:latest $DOCKER_USERNAME/two-tier-flask-app:latest
                        docker push $DOCKER_USERNAME/two-tier-flask-app:latest
                    """
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down"
                sh "docker-compose up -d --build flask-app"
            }
        }
    }
}
