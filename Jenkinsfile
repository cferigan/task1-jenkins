pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                sh '''
                docker build -t cferigan/task1-jenkins .
                '''

            }

        }
        
        stage('Push') {

            steps {

                sh '''
                docker push cferigan/task1-jenkins
                '''

            }

        }
          stage('Deploy') {

            steps {

                sh '''
                docker stop task1
                docker rm task1
                docker run -d -p 80:5500 --name task1 cferigan/task1-jenkins
                '''

            }

        }

    }

}