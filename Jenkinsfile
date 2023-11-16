pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                sh '''
                docker build -t cferigan/task1-jenkins .
                docker build -t cferigan/task1-nginx nginx
                '''

            }

        }
        
        stage('Push') {

            steps {

                sh '''
                docker push cferigan/task1-jenkins
                docker push cferigan/task1-nginx
                '''

            }

        }
          stage('Deploy') {

            steps {

                sh '''
                ssh jenkins@connor-deploy <<EOF
                docker pull cferigan/task1-jenkins
                docker pull cferigan/task1-nginx
                docker stop nginx && echo "Stopped nginx" || echo "nginx is not running"
                docker rm nginx && echo "removed nginx" || echo "nginx does not exist"
                for i in {1..3}; do
                docker stop flask-app-${i} && echo "Stopped flask-app" || echo "flask-app is not running"
                done
                for i in {1..3}; do
                docker rm flask-app-${i} && echo "removed flask-app" || echo "flask-app does not exist"
                done
                docker network rm task1-net && echo "task1-net removed" || echo "network already removed"
                docker network create task1-net
                for i in {1..3}; do
                docker run -d -p 80:5500 --name flask-app-${i} --network task1-net cferigan/task1-jenkins
                done
                docker run -d --name nginx --network task1-net -p 81:80 cferigan/task1-nginx
                '''

            }
 
        }

    }

}