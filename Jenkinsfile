pipeline{
    environment {
        imagename = "oussama24/backendapp"
        registryCredential = "dockerhub_credentials"
        dockerImage = 'backendapp'
        
    }
    agent any
    stages{
       
        stage("build"){
            
            steps{
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        
}
