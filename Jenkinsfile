pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven 3"
        jdk "JDK 21"
    }
    
    environment {
        SCANNER_HOME= tool 'SonarScanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/harshtyagi174/calculator-app.git'
                    
                }
                }
            }
        
        stage('Build'){
             steps {
                script {
                    bat "mvn clean compile"
                    
                
                }
            }
            
        }
        
        
        stage('Unit tests'){
             steps {
                script {
                    bat "mvn test"
                    
                
                }
            }
            
        }
        
        stage('Quality Analysis'){
             steps {
                script {
                    withSonarQubeEnv('Token_Sonar'){
                        bat "mvn sonar:sonar"
                    }
                    
                
                }
            }
            
        }
        
        
        stage('Deploy'){
             steps {
                echo "Deploying to prod"
                    
                
                }
            }
            
        
    }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    //archiveArtifacts 'target/*.jar'
                }
            }
        }



