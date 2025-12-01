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
        stage('SonarQube Analysis') {
            steps {
                echo "🔍 Analyzing code quality with SonarQube..."
                withSonarQubeEnv('sonarqube') {
                    sh """
                    mvn sonar:sonar \
                      -Dsonar.projectKey=${PROJECT_KEY} \
                      -Dsonar.projectName='${PROJECT_NAME}' \
                      -Dsonar.host.url=${SONARQUBE_URL} \
                      -Dsonar.java.binaries=target/classes \
                      -Dsonar.sources=src/main/java \
                      -Dsonar.tests=src/test/java \
                      -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                      -Dsonar.test.inclusions='**/*UnitTest.java'
                    """
                }
            }
        }

    } post {
        always {
            
            mail to: 'rbenomrane15@gmail.com',
                 subject: "Pipeline finished: ${currentBuild.currentResult}",
                 body: "Job finished with result: ${currentBuild.currentResult}"
        }
    }

}

