pipeline{
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Images'){
            steps{
                sh "docker build . -t  dockerusername/repo:${DOCKER_TAG}"
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker.hub', variable: 'DockerHub')]) {
                     sh "docker login -u dockerusername -p ${DockerHub}"
                     sh "docker push dockerusername/repo:${DOCKER_TAG}"
                }
            }
        }
        stage('deply to k8s'){
            steps{
                sh "chmod +x changetag.sh"
                sh "./changetag.sh ${DOCKER_TAG}"
                kubernetesDeploy(
                    configs: 'node-app-pod.yaml',
                    kubeconfigId: 'ekscluseter',
                    enableConfigSubstitution: true
                    )       
            }
        }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}



