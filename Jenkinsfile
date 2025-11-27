pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Save Git Info') {
            steps {
                script {
                    def commit = sh(script: 'git log -1 --pretty=format:"%H"', returnStdout: true).trim()
                    def author = sh(script: 'git log -1 --pretty=format:"%an"', returnStdout: true).trim()
                    def message = sh(script: 'git log -1 --pretty=format:"%s"', returnStdout: true).trim()
                    def date = sh(script: 'git log -1 --pretty=format:"%ad"', returnStdout: true).trim()

                    sh """
                    echo "Commit: ${commit}" > github-log.txt
                    echo "Author: ${author}" >> github-log.txt
                    echo "Message: ${message}" >> github-log.txt
                    echo "Date: ${date}" >> github-log.txt
                    """

                    echo "GitHub info saved to github-log.txt"
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'github-log.txt'
            mail to: 'rbenomrane15@gmail.com',
                 subject: "Pipeline finished: ${currentBuild.currentResult}",
                 body: "Job finished with result: ${currentBuild.currentResult}"
        }
    }
}
