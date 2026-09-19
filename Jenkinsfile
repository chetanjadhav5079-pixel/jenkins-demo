pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Build stage started'
                echo "Selected Environment: ${params.ENV}"
                sh 'cat app.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Test stage started'
                echo 'Test passed successfully'
            }
        }

        stage('Deploy to DEV') {
            when {
                expression {
                    params.ENV == 'dev'
                }
            }
            steps {
                echo 'Deploying application to DEV environment'
            }
        }

        stage('Deploy to QA') {
            when {
                expression {
                    params.ENV == 'qa'
                }
            }
            steps {
                echo 'Deploying application to QA environment'
            }
        }

        stage('Deploy to PROD') {
            when {
                expression {
                    params.ENV == 'prod'
                }
            }
            steps {
                echo 'Deploying application to PROD environment'
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