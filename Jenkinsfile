pipeline {
    agent any

    tools {
        maven 'Maven 3'
        jdk 'JDK 21'
    }

    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_SCANNER_HOME = tool 'SonarScanner'
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
        skipStagesAfterUnstable()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/harshtyagi174/calculator-app.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarScanner') {
                    bat """
                        ${SONAR_SCANNER_HOME}\\bin\\sonar-scanner.bat ^
                        -Dsonar.projectKey=calculator-app^
                        -Dsonar.sources=src ^
                        -Dsonar.tests=src/test ^
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Upload to Artifactory') {
            steps {
                bat 'mvn deploy'
            }
        }
    }
}


