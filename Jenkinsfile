pipeline{
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Images'){
            steps{
                sh "docker build . -t  docker/repo:${DOCKER_TAG}"
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker.hub', variable: 'DockerHub')]) {
                     sh "docker login -u dockerusername -p ${DockerHub}"
                     sh "docker push dockerusername/cicdtesting:${DOCKER_TAG}"
                }
            }
        }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

