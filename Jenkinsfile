pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'docker build -t hazemhashem100/web:v${BUILD_NUMBER} .'
            } 
        }
        stage('push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh 'docker login -u ${USERNAME} -p ${PASSWORD}'
                    sh "docker push hazemhashem100/web:v${BUILD_NUMBER}"
                }
            }
        }
        stage('CD') {
            steps {
                sh "sed -i 's/latest/${BUILD_NUMBER}/' appdeploy.yml "
                sh "kubectl apply -f namespace.yml"
                sh "kubectl apply -f . "
            }
        }
    }
}
