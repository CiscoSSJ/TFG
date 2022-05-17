pipeline{
    agent any
    tools {
      maven 'Maven3'
    }
    environment {
      DOCKER_TAG = getVersion()
    }
    stages{
        stage('GitHub'){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/CiscoSSJ/TFG'
            }
        }
        
        stage('Maven'){
            steps{
                sh "mvn clean package"
            }
        }
        stage('Docker Build'){
            steps{
                sh "docker build . -t ciscossj/ciscoapp:${DOCKER_TAG} "
            }
        }
        stage('Docker Hub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker_hub', variable: 'dockerHubPasswd')]) {
                    sh "docker login -u ciscossj -p ${dockerHubPasswd} "
                }
                sh "docker push ciscossj/ciscoapp:${DOCKER_TAG}"
            }
        }
        stage('Docker Deploy'){
            steps{
                ansiblePlaybook credentialsId: 'dev', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
    }
}

def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
