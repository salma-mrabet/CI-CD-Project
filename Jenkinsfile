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
        // stage('Static code analysis'){
            
        //     steps{
                
        //         script{
                    
        //             withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
        //                 sh 'mvn clean package sonar:sonar'
        //             }
        //            }
                    
        //         }
        //     }
        //     stage('Quality Gate Status'){
                
        //         steps{
                    
        //             script{
                        
        //                 waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
        //             }
        //         }
        //     }
         }
        
}