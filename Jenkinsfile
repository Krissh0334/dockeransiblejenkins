pipeline{
    agent any 
    tools {
      maven 'maven'
}
    stages{
        stage('SCM'){
            steps{
                git credentialsId: 'krisshgithub',
                    url: 'https://github.com/Krissh0334/dockeransiblejenkins.git'
             
            }
        }
        
        stage('maven build'){
            steps{
                sh "mvn clean package"
            }
        }
    
        stage('docker build'){
            steps{
                sh "docker build -t sivaram0334/ramapp:0.0.1 ."
            }
        }    
    
        stage('dockerhub push'){
            steps{
             withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpwd')]) {
                sh "docker login -u sivaram0334 -p ${dockerhubpwd}"
            }
                sh "docker push sivaram0334/ramapp:0.0.1 "
            }
        }    
       
        stage('docker deploy'){
            steps{
                ansiblePlaybook credentialsId: 'dev', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
    }    
}    

def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
