pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/uialberto/netcore50-api-test.git'
                
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
                bat(script: 'docker build -t uialberto/servicenet5jenkin .', returnStdout: true);
                bat(script: 'docker push uialberto/servicenet5jenkin', returnStdout: true);
            }
        }
        stage('Deploy Dev') {
            steps {
                bat(script: 'az login --service-principal -u 895ee115-4b74-4ffc-9eba-928054ecf3fc -p qvND3~4NaO_pbaz38IG6cJCz2rX_Dj-prb --tenant b9a8f4e8-64dd-474a-be66-0dce5463c288', returnStdout: true);
                bat(script: 'az account set --subscription "AlbertoDevelop"', returnStdout: true);
                bat(script: 'az container restart --name devopsuialberto --resource-group RG-Test-Dev', returnStdout: true);                                
            }
        }
        stage('Deploy Prod') {
            steps {                
                bat(script: 'az aks get-credentials --resource-group  RG-Test-Dev  --name cluster-devops-ui & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'kubectl config use-context cluster-devops-ui --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'Kubectl delete --all pods --kubeconfig=%KUBE_PATH_CONFIG% & kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            }
        }
    }
}
