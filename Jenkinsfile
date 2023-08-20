pipeline{
    
    agent any 
    
    stages {
        // This stage performs a Git checkout of the 'main' branch from the specified repository.
        // The code from the repository will be used for further build and deployment processes.
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/salma-mrabet/application'                
                
                }
            }
        }
        // This stage's purpose is to automate the execution of unit tests 
        // By running unit tests as part of the Jenkins pipeline, we can quickly identify if any code changes introduced defects or broke existing functionality.
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    bat 'mvn test'
                }
            }
        }

        // The purpose of this stage is to automate the execution of integration tests using the mvn verify command while skipping unit tests
        // Integration tests help identify issues that may arise when different parts of the application are combined, providing insights into how well the system components interact.
        // This stage is likely to involve more complex scenarios compared to unit testing.
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    bat 'mvn verify -DskipUnitTests'
                }
            }
        }

        // The purpose of this stage is to automate the execution of the Maven build process using the mvn clean install command. 
        //  This ensures that the codebase is built from the source, tested, and packaged into a usable form.
        // The Maven build process encompasses several stages, including cleaning the project (removing old build artifacts), compiling source code, running tests, generating project documentation, packaging the application into a distributable format, and installing it in the local Maven repository.
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    bat 'mvn clean install'
                }
            }
        }

        //  Static code analysis is a method of analyzing code without executing it. It focuses on identifying potential defects, vulnerabilities, and code quality issues by analyzing the source code's structure, syntax, and patterns.
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                      bat 'mvn clean package sonar:sonar'
                    }
                }
                    
            }
        }
            // A Quality Gate is a set of conditions that code must meet to be considered of acceptable quality. 
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

        stage('Docker Image Build'){

            steps{

                script{

                    // building a Docker image and giving it a tag based on the Jenkins job name and the build number.

                    bat 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                    bat 'docker image tag $JOB_NAME:v1.$BUILD_ID salmamrabet/$JOB_NAME:v1.$BUILD_ID'
                    bat 'docker image tag $JOB_NAME:v1.$BUILD_ID salmamrabet/$JOB_NAME:latest'



                
                }
            }
        }

    }
        
}