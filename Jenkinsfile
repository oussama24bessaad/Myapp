pipeline{
    environment {
        imagename = "oussama24/backendapp"
        registryCredential = "dockerhub_credentials"
        dockerImage = 'backendapp'
    }
    agent any
    stages{

            
        
        stage('SonarQube analysis') {
                    
            steps{
                script {
               scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('sonarqube-server') { 
        
                       sh "${scannerHome}/bin/sonar-scanner"
                     
                    }
                }         
            }
        }
        
        stage("build"){
            
            steps{
                sh 'npm install'
                sh 'docker --version'
            }
        }
      
        stage("docker-build"){
            steps{  
                    
                    script {
                    dockerImage = docker.build imagename   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        stage("deploy"){
            steps{
            script {
                    kubernetesDeploy(configs: "./Kubernetes/backend-deployment.yaml", kubeconfigId: "k8s")
                    kubernetesDeploy(configs: "./Kubernetes/mongo-statefullset.yaml", kubeconfigId: "k8s")  
                    kubernetesDeploy(configs: "./Kubernetes/ingress.yaml", kubeconfigId: "k8s")  
                    kubernetesDeploy(configs: "./Kubernetes/frontend-deployment.yaml", kubeconfigId: "k8s")
        }
      }
        }  
    }
}
