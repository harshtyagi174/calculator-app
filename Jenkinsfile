pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven"
        jdk "OpenJDK"
    }
    
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    git url: 'https://github.com/harshtyagi174/calculator-app.git'
                    
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
        
        stage('Qquality Analysis'){
             steps {
                script {
                    withSonarQubeEnv(installationName: 'Sonar', credentialsId: 'Token_Sonar'){
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
