pipeline {
    agent any
    environment {
        IMAGE_NAME = "thethymca/next-node-js-app:${BUILD_NUMBER}"
        DOCKER_REGISTRY = "https://index.docker.io/v1/"
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
                    echo 'Image built done'
                    //echo 'Image built, Now pushing to docker registry....'
                    
                    // docker.withRegistry('https://index.docker.io/v1','docker-credentials') {
                    // docker.withDockerRegistry('https://index.docker.io/v1','docker-credentials') {
                    //    sh "docker push ${IMAGE_NAME}"
                    // }
                    //echo 'Docker image pushed successfully!'
                }
            }
        }
        stage('docker push') {
            steps {
                echo 'starting to push image here ...'
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME
                    '''
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
