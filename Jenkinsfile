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
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=https://18.207.190.234:9000 -Dsonar.login=squ_6708a10764879bd480742ab1c011b591923b9e19  -Dsonar.projectName=eart \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=ekart '''
                
            }
            
        }
        stage('Build app') {
            steps {
                sh "mvn clean install"
                
            }
            
        }
        stage('Build and push docker image') {
            steps {
                script {
                    withDockerRegistry(crendentialsID: 'docker' toolName:'docker'){
                        sh "docker buid -t ekart:latest -f docker/Dockerfile . "
                        sh " docker tag ekart:latest harsha7633/ekart:latest"
                        sh "docker push harsha7633/ekart:latest"
                    }
                }
            }
        }
        
    }
}
