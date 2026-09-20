pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Build stage started'
                echo "Selected Environment: ${params.ENV}"
                sh 'cat index.html'
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
                echo "Building Docker image: ${BUILD_NUMBER}"

                sh """
                    docker build -t jenkins-demo:${BUILD_NUMBER} .
                """
            }
        }

        stage('Push to ECR') {
            steps {
                echo "Pushing Docker image ${BUILD_NUMBER} to ECR..."

                sh """
                    aws ecr get-login-password --region us-east-2 | \
                    docker login --username AWS --password-stdin \
                    426550965418.dkr.ecr.us-east-2.amazonaws.com

                    docker tag \
                    jenkins-demo:${BUILD_NUMBER} \
                    426550965418.dkr.ecr.us-east-2.amazonaws.com/jenkins-demo:${BUILD_NUMBER}

                    docker push \
                    426550965418.dkr.ecr.us-east-2.amazonaws.com/jenkins-demo:${BUILD_NUMBER}
                """
            }
        }

        stage('Deploy to DEV') {
            when {
                expression {
                    params.ENV == 'dev'
                }
            }

            steps {
                echo "Deploying build ${BUILD_NUMBER} to DEV"

                sh """
                    kubectl -n default set image deployment/jenkins-demo \
                    jenkins-demo=426550965418.dkr.ecr.us-east-2.amazonaws.com/jenkins-demo:${BUILD_NUMBER}

                    kubectl -n default rollout status deployment/jenkins-demo \
                    --timeout=180s
                """
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