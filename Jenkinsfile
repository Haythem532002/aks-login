pipeline {
    agent any
    environment {
        AZURE_RESOURCE_GROUP = "book-store-ressource"
        AZURE_AKS_CLUSTER_NAME = "book-store-aks"
        AZURE_TENANT_ID = "dbd6664d-4eb9-46eb-99d8-5c43ba153c61" 
    }
    stages {
        stage('checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/Haythem532002/aks-login.git'
            }
        }
        stage('Login to Azure') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'azure-service-principal', usernameVariable: 'AZURE_CLIENT_ID', passwordVariable: 'AZURE_CLIENT_SECRET')]) {
                    sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID'
                }
            }
        }

        stage('Get AKS Credentials') {
            steps {
                sh 'az aks get-credentials --resource-group $AZURE_RESOURCE_GROUP --name $AZURE_AKS_CLUSTER_NAME --overwrite-existing'
            }
        }

        stage('Deploy to AKS') {
            steps {
                sh 'kubectl run test-pod --image=nginx --restart=Never'
                sh 'sleep 10'
                sh 'kubectl get pods'
            }
        }
    }
}
