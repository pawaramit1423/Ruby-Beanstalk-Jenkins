pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pawaramit1423/Ruby-Beanstalk-Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh '/opt/elasticbeanstalk/.rbenv/shims/gem install bundler'
                sh '/opt/elasticbeanstalk/.rbenv/shims/bundle install'
            }
        }
        
        stage('Package Application') {
            steps {
                sh 'zip -r app.zip .'
            }
        }

        stage('Deploy to AWS Beanstalk') {
            environment {
                APPLICATION_NAME = 'ruby'
                ENVIRONMENT_NAME = 'Ruby-env'
                VERSION_LABEL = "${env.BUILD_NUMBER}"
                BUCKET_NAME = 'elasticbeanstalk-us-east-1-17....'
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
