pipeline {
    agent any
    tools {
        maven 'Maven3'     
        jdk 'jdk17'       
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
        stage('Run Tests') {
            steps {
                // Run only your 3 test classes
                sh 'mvn test -Dtest=EnrollementManagementTests,DepartementsManagementTests,StudentManagementApplicationTests'
            }
        }
       
    } post {
        always {
            archiveArtifacts artifacts: 'github-log.txt'
            mail to: 'rbenomrane15@gmail.com',
                 subject: "Pipeline finished: ${currentBuild.currentResult}",
                 body: "Job finished with result: ${currentBuild.currentResult}"
        }
    }

}

