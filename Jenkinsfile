pipeline {
    agent any

    tools {
        maven 'Maven 3.9.6'
        jdk 'JDK 17'
    }

    environment {
        SONARQUBE = 'SonarQube'
        SCANNER_TOOL = 'SonarScanner'
        ARTIFACTORY = 'Artifactory'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/harshtyagi174/calculator-app.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat "\"%SCANNER_TOOL%\\bin\\sonar-scanner.bat\""
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
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

