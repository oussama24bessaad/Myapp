pipeline{
    environment {
        registryCredential = "dockerhub_credentials"
        imagenameback = "oussama24/backendapp"
        dockerImageback = 'backendapp'
        imagenamefront = "oussama24/frontendapp"
        dockerImagefront = 'frontendapp'
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
                    dockerImageback = docker.build imagenameback   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImageback.push("$BUILD_NUMBER")
                    dockerImageback.push('latest')
                    }
                         script {
                    dockerImagefront = docker.build imagenamefront   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImagefront.push("$BUILD_NUMBER")
                    dockerImagefront.push('latest')
                    }
                }
            }
        }
        stage('Deploy App') {
    steps {
        withCredentials([
            string(credentialsId: 'my_kubernetes', variable: 'api_token')
            ]) {
             sh 'kubectl --token $api_token --server https://192.168.49.2:8443 --insecure-skip-tls-verify=true apply -f ./Kubernetes '
               }
            }
           }
    }
}
