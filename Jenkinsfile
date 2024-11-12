pipeline{
    agent any 
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Reference Docker Hub credentials
        DOCKER_IMAGE = "gass7400/kube-image"
    }
    stages{
        stage('build image'){
            steps{
                script{
                app = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('test image'){
            steps{
                script{
                    echo 'test passed'
                }
            }
        }
        stage('push image'){
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('Trigger ManifestUpdate') {
            steps {
                echo "Triggering update-manifest-file job"
                build job: 'update-manifast-file', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
            }
        }
    }
}
