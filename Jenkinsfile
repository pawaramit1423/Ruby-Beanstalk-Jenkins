pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/KunalTi/ReactJS-Beanstalk-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh 'gem install bundler'
                sh 'bundle install'
            }
        }
        
        stage('Package Application') {
            steps {
                sh 'zip -r app.zip .'
            }
        }

        stage('Deploy to AWS Beanstalk') {
            environment {
                APPLICATION_NAME = 'rubdi'
                ENVIRONMENT_NAME = 'rundi-env-1'
                VERSION_LABEL = "${env.BUILD_NUMBER}"
                BUCKET_NAME = 'elasticbeanstalk-us-east-1-176295807911'
                BUCKET_KEY = 'app.zip'
            }
            
            steps {
                script {
                    sh "aws s3 cp app.zip s3://${BUCKET_NAME}/${BUCKET_KEY}"
                    sh "aws elasticbeanstalk create-application-version --application-name ${APPLICATION_NAME} --version-label ${VERSION_LABEL} --source-bundle 'S3Bucket=${BUCKET_NAME},S3Key=${BUCKET_KEY}'"
                    sh "aws elasticbeanstalk update-environment --application-name ${APPLICATION_NAME} --environment-name ${ENVIRONMENT_NAME} --version-label ${VERSION_LABEL}"
                }
            }
        }
    }
}
