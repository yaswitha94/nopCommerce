pipeline {
    agent any
    stages {
        stage('Source Code') {
            steps {
                git url: 'https://github.com/yaswitha94/nopCommerce.git', 
                branch: 'develop'
            }
            
        }
       stage("Build and Push"){
            steps{
                  sh 'docker image build -t yaswithaa/nop:1.0 .'
                  sh 'docker image push yaswithaa/nop:1.0'
            }
       }
        stage('cluster') {
            steps{
                sh 'az group create --name fornop --location eastus'
                sh 'az aks create -g fornop -n myAKS --enable-managed-identity --node-count 1 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys'
                sh 'sudo az aks install-cli'
                sh 'az aks get-credentials --resource-group fornop --name myAKS'
            }
        }
       stage('deploy'){
        steps{
            sh 'kubectl apply -f deploy.yaml'
            
        }
       }
    }
}    
