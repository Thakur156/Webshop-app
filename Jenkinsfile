pipeline {
    agent any
    
    tools {
        jdk 'jdk17'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('git Checkout') {
            steps {
                git 'https://github.com/Thakur156/Webshop-app'
            }
        }
        
        stage('Clojure DEPendency Check') {
            steps {
                sh "clj-holmes fetch-rules"
                sh "clj-holmes scan -p ./"
            }
        }
        
        stage('Trivy FS Scan') {
            steps {
                sh "trivy fs ."
            }
        }
        
        stage('SONARQUBE Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh """$SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=Clojure-App \
                        -Dsonar.java.binaries=./ \
                        -Dsonar.projectKey=Clojure-App
                    """
                }
            }
        }
        
        stage('Build & Deploy') {
            steps {
                sh "docker-compose up -d"
            }
        }
        
    }
}

