pipeline {
    agent any
    tools {
        maven 'Maven3'     
        jdk 'JDK17'       
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
    }
}

