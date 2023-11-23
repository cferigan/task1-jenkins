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
        stage('Staging Deploy') {
            steps {
                sh '''
                kubectl apply -f nginx-config.yaml --namespace staging
                kubectl apply -f app-manifest.yaml --namespace staging
                kubectl apply -f nginx-pod.yaml --namespace staging
                '''
            }
        }
        stage('Quality Check') {
            steps {
                sh '''
                sleep 50
                export STAGING_IP=\$(kubectl get svc -o json --namespace staging | jq '.items[] | select(.metadata.name == "nginx") | .status.loadBalancer.ingress[0].ip' | tr -d '"')
                pip3 install requests
                python3 test-app.py
                '''
            }
        }
          stage('Prod Deploy') {

            steps {

                sh '''
                kubectl apply -f nginx-config.yaml --namespace prod
                kubectl apply -f app-manifest.yaml --namespace prod
                kubectl apply -f nginx-pod.yaml --namespace prod
                sleep 60
                kubectl get services --namespace prod
                '''

            }
 
        }

    }

}