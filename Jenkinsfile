pipeline {
    agent any
    tools{
        jdk 'jdk'
        maven 'maven'
    }
    environment{
        SCANNER_HOME=  tool 'sonarqube'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git  branch : 'master',  url: 'https://github.com/harsha5401/ekart.git'
                
            }
            
        }
        stage('compile code') {
            steps {
                sh "mvn clean compile"
                
            }
            
        }
        stage('sonar scanner') {
            steps {
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=https://18.207.190.234:9000 -Dsonar.login=squ_5da030596d8a9e1365902c4927de801f2177b155  -Dsonar.projectName=eart \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=ekart '''
                
            }
            
        }
        stage('Build app') {
            steps {
                sh "mvn clean install -DskipTests=true"
                
            }
            
        }
        stage('Build and push docker image') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker1', variable: 'docker1')]) {
                       sh 'docker login -u harsha7633 -p ${docker1}'
                        sh "docker buid -t ekart:latest -f docker/Dockerfile . "
                        sh " docker tag ekart:latest harsha7633/ekart:latest"
                        sh "docker push harsha7633/ekart:latest"
                    }
                }
            }
        }
        
    }
}
