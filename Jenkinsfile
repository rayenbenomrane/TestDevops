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
                echo "🧪 Running selected test classes..."
                sh 'mvn test -Dtest=EnrollmentManagementTests,DepartementsManagementTests,StudentManagementApplicationTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "🔍 Running SonarQube analysis..."
                // 'sonar' must match the name of your SonarQube server in Jenkins
                withSonarQubeEnv('sonar') {
                    sh """
                        mvn sonar:sonar \
                          -Dsonar.projectKey=rayen \
                          -Dsonar.projectName='TestDevops-main' \
                          -Dsonar.java.binaries=target/classes \
                          -Dsonar.sources=src/main/java \
                          -Dsonar.tests=src/test/java \
                          -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                          -Dsonar.test.inclusions='**/EnrollmentManagementTests.java,**/DepartementsManagementTests.java,**/StudentManagementApplicationTests.java'
                    """
                }
            }
        }
        stage('Get Spring Boot Logs') {
    steps {
        sh 'kubectl logs springboot-fc7f55c9c-c7pk8'
    }
}

    }

    post {
        always {
            mail to: 'rbenomrane15@gmail.com',
                 subject: "Pipeline finished: ${currentBuild.currentResult}",
                 body: "Job finished with result: ${currentBuild.currentResult}"
        }
    }
}
