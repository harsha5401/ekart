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
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://localhost:9010/ -Dsonar.login=squ_8ad5af70710a70736e7e58ac2367c4896cd859ae  -Dsonar.projectName=eart \
                -Dsonar.java.binaries= . \
                -Dsonar.ProjectKey=ekart '''
                
            }
            
        }
    }
}
