pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
               git branch: 'main', url: 'https://github.com/miguelampudia/MS-DOTNET6.git'
            }
        }
        stage('Build Net6.0') {
            steps {
                bat(script: 'dir' , returnStdout:true);
                bat(script: 'dotnet restore' , returnStdout:true);
                bat(script: 'dotnet build' , returnStdout:true);
                bat(script: 'dotnet test' , returnStdout:true);
            }
        }
        stage('Docker Build') {
            steps {
               bat(script: 'docker build -t mampudia/dotnet6:v4 .' , returnStdout:true);
               bat(script: 'docker push  mampudia/dotnet6:v4' , returnStdout:true); 
            }
        }
        stage('Deploy AKS') {
            steps {
               bat(script: 'kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%' , returnStdout:true);
               bat(script: 'kubectl rollout restart deployment app-deployment --kubeconfig=%KUBE_PATH_CONFIG%' , returnStdout:true);
            }
        }
    }
}
