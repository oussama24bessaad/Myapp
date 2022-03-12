pipeline{
    environment {
        imagename = "oussama24/backendapp"
        registryCredential = "dockerhub_credentials"
        dockerImage = 'backendapp'
        
    }
    agent any
    stages{
        stage("SonarQube analysis"){
            steps{
                script {
                    scannerHome = tool 'SonarQube Scanner 4.6.2.2472'
                }
                    withSonarQubeEnv("SonarQube Scanner") {
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=Myapp \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=admin \
                        -Dsonar.password=admin007"
                    } 
                }
            }
        }
        stage("build"){
            
            steps{
                sh 'npm install'
                sh 'npm run build'
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
                    kubernetesDeploy(configs: "./Kubernetes/backend-deployment.yaml", kubeconfigId: "kubernetes")
                    kubernetesDeploy(configs: "./Kubernetes/mongo-statefullset.yaml", kubeconfigId: "kubernetes")  
                    kubernetesDeploy(configs: "./Kubernetes/ingress.yaml", kubeconfigId: "kubernetes")  
                    kubernetesDeploy(configs: "./Kubernetes/frontend-deployment.yaml", kubeconfigId: "kubernetes")
        }
      }
        }  

}
}
