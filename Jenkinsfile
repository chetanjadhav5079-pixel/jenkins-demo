pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Build stage started'
                sh 'cat app.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Test stage started'
                echo 'Test passed successfully'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy stage started'
                echo 'Application deployment simulated'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}