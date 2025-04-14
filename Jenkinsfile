pipeline {
    agent any
    environment {
        IMAGE_NAME = "thethymca/next-node-js-app:${BUILD_NUMBER}"
    }
    stages {
        stage('checkout code') {
            steps {
                git branch: 'master', url: 'https://github.com/harkaur02/docker-jenkins-argocd-using-next_js.git'
            }
        }
        stage('docker build') {
            steps {
                script{
                    sh "docker build -t ${IMAGE_NAME} ."
                    echo 'Image built, Now pushing to docker registry....'
                    docker.withRegistry('https://index.docker.io/v1','docker-credentials') {
                        sh "docker push ${IMAGE_NAME}"
                    }
                    ehco 'Docker image pushed successfully!'
                }
            }
        }       
    }
    post {
        success {
            build job: 'k8s-manifest-pipeline'
        }
    }    
}
