pipeline{
    environment {
        imagenameback = "oussama24/backendapp"
        imagenamefront = "oussama24/frontendapp"
        imagenamemongo = "mongo"
        registryCredential = "dockerhub_credentials"
        dockerImage = 'frontendapp'
        
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
                        -Dsonar.projectKey=oussamaDevops \
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
//         stage('Login') {

// 			steps {
// 				sh 'echo $registryCredential_PSW | docker login -u $registryCredential_USR --password-stdin'
// 			}
// 		}
        
        stage("Backend_docker-build"){
            steps{
                    script {
                    dockerImage = docker.build imagenameback   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        stage("Frontend_docker-build"){
            steps{
                    script {
                    dockerImage = docker.build imagenamefront   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        stage("Mongo_docker-build"){
            steps{
                    script {
                    dockerImage = docker.build imagenamemongo   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        
}
