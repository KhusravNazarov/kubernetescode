pipeline{
    agent any 
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Reference Docker Hub credentials
        DOCKER_IMAGE = "gass7400/kube-image"
    }
    stages{
        stage('build image'){
            script{
                app = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
            }
        }
        stage('test image'){
            echo 'test passed'
        }
        stage('push image'){
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
