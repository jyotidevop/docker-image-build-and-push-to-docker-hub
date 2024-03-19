pipeline {
    agent any 
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/jyotidevop/docker-image-build-and-push-to-docker-hub.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker image build . -t $JOB_NAME:$BUILD_ID"
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker image tag $JOB_NAME:$BUILD_ID ${env.dockerHubUser}/$JOB_NAME:$BUILD_ID"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/$JOB_NAME:$BUILD_ID"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker container run -d -it -p 80:80 $JOB_NAME:$BUILD_ID "
                
            }
        }
}
}
