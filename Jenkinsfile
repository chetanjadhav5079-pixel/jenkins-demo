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

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t jenkins-demo:1.0 .'
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

        stage('Credentials Test') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'demo',
                        usernameVariable: 'USERNAME',
                        passwordVariable: 'PASSWORD'
                    )
                ]) {
                    sh 'echo "Username: $USERNAME"'
                }
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