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
                kubectl apply -f .
                '''

            }
 
        }

    }

}