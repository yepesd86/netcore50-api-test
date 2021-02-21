pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Stywar/netcore50-api-test'
                
            }
        }
        stage('Build') {
            steps {
                bat(script: 'dir', returnStdout: true);
                bat(script: 'dotnet restore', returnStdout: true);
                bat(script: 'dotnet build', returnStdout: true);
                bat(script: 'dotnet test', returnStdout: true);
            }
        }
        stage('Docker') {
            steps {
                bat(script: 'docker login --username %UsernameDockerHub% --password %PasswordDocker%', returnStdout: true);
                bat(script: 'docker build -t antony0618/servicenet5 .', returnStdout: true);
                bat(script: 'docker push antony0618/servicenet5', returnStdout: true);
            }
        }
        stage('Deploy Dev') {
            steps {
                bat(script: 'az login --service-principal --username 60445a5a-4c7d-404e-96a0-0d5c83c4978f --password wP_yGAcZKPUZQBTd.ezSLAzpNAzY-ZcZ6l --tenant 74343d69-5375-4c7a-8cc9-08986488c964', returnStdout: true);
                bat(script: 'az account set --subscription "StywarV"', returnStdout: true);
                bat(script: 'az container restart --name micro5testservice --resource-group aforo255Devops', returnStdout: true);
                //bat(script: 'az container delete --resource-group aforo255Devops --name micro5testservice --yes', returnStdout: true);
                //bat(script: 'az container create --resource-group aforo255Devops --name micro5testservice --image antony0618/servicenet5:latest --dns-name-label micro5testservice --ports 80', returnStdout: true);
            }
        }
        stage('Deploy Prod') {
            steps {
                //bat(script: 'az aks get-credentials --resource-group aforo255Devops --name aforo255jenkins & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'az aks get-credentials --resource-group  aforo255Devops  --name cluster-Devops & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'kubectl config use-context cluster-Devops --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'Kubectl delete --all pods --kubeconfig=%KUBE_PATH_CONFIG% & kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            }
        }
    }
}