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
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub_cred', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh """
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker tag two-tier-flask-app $DOCKER_USERNAME/two-tier-flask-app:latest
                        docker push $DOCKER_USERNAME/two-tier-flask-app:latest
                    """
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh """
                     kubectl apply -f k8s/mysql-deployment.yml 
                     kubectl apply -f k8s/mysql-svc.yml
                     kubectl apply -f k8s/two-tier-app-deployment.yml
                     kubectl apply -f k8s/two-tier-app-svc.yml
                """
            }
        }
        stage('Port Forward Service') {
            steps {
                sh '''
                    export KUBECONFIG=/var/lib/jenkins/.kube/config
                    nohup kubectl port-forward svc/two-tier-app-service 5000:80 --address=0.0.0.0 > port-forward.log 2>&1 &
                '''
            }
        }

    }
}
