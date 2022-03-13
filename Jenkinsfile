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
        stage('Deploy App') {
    steps {
        withCredentials([
            string(credentialsId: 'my_kubernetes', variable: 'api_token')
            ]) {
             sh 'kubectl --token $api_token --server https://192.168.49.2:8443 --insecure-skip-tls-verify=true apply -f ./Kubernetes/backend-deployment.yaml '
               }
            }
           }
    }
}
