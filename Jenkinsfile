pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
git branch: 'main', url: 'https://github.com/salma-mrabet/application'                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    bat '"C:\\Users\\Moad\\Downloads\\apache-maven-3.9.4-bin\\apache-maven-3.9.4\\bin\\mvn" test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    bat '"C:\\Users\\Moad\\Downloads\\apache-maven-3.9.4-bin\\apache-maven-3.9.4\\bin\\mvn" verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    bat '"C:\\Users\\Moad\\Downloads\\apache-maven-3.9.4-bin\\apache-maven-3.9.4\\bin\\mvn" clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                      bat 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            // stage('Quality Gate Status'){
                
            //     steps{
                    
            //         script{
                        
            //             waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
            //         }
            //     }
            // }
            stage('Upload war file to Nexus'){

                steps{

                    script{

                        def readPomVersion = readMavenPom file : 'pom.xml'

                        nexusArtifactUploader artifacts: 
                        [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], 
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: 'localhost:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'application-release', 
                        version: "${readPomVersion}"
                    }
                }
            }
         }
        
}