
pipeline {
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven3.9'
    }
     stages{
        stage("Git Checkout"){
            steps{
                git branch: 'master', changelog: false, poll: false, url: 'https://github.com/s7478/Intelligence-Transaction-Payment-Application.git'
            }
        }
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        stage("Sonarqube Scanner"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'Sonar-server') {
                    sh 'mvn sonar:sonar'   
                    }
                 }
            }
        }
        stage("Build"){
            steps{
                sh " mvn clean install"
            }
        }
       stage("Docker Build and Image Push"){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                       sh "docker build -t shantanu441/loginwebappseven ."
                       sh "docker push shantanu441/loginwebappseven:latest"
                    }
                }
            }     
        }
        stage("TRIVY"){
            steps{
                sh "trivy image shantanu441/loginwebappseven:latest > trivyimage.txt"
            }
        } 
        stage("Deploy using Docker container"){
            steps{
                sh "docker run -d --name=loginwebseven190 -p 8084:8080 shantanu441/loginwebappseven:latest"
            }
        }       
}
}
