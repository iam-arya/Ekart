pipeline {
    agent any 
    
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/iam-arya/Ekart.git'
            }
        }
        
       stage('COMPILE') {
            steps {
                sh "mvn clean compile -DskipTests=true"
            }
        }
      
        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('sonar-server'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Shopping-Cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               }
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
        
        stage('Deploy to nexus') {
            steps {
                script{
                 withMaven(globalMavenSettingsConfig: '1991b344-bfd3-462c-9f65-e08d1078f6c2', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                        
                        sh "mvn deploy -DskipTests=true"
                        
                    }
                }
            }
        }
        
        
    }
}
