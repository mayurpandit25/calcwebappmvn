pipeline {
    agent any

    tools {
        maven 'maven1'
    }

    environment {
        my-aws-credential = credentials('aws-access')
    }

    stages {
        stage('checkout') {
            steps {
                git 'https://github.com/mayurpandit25/calcwebappmvn.git'
                echo 'clonning is successful'
            }
        }

        stage('Build') {
            steps {
                sh '''
                ls -la
                mvn clean package
                ls -la
                '''
            }

            post {
                success {
                    archiveArtifacts 'target/*.war'
                    sh 'ls -la'
                }
            }
        }

        stage('artifact to s3') {
            steps {
                sh 'aws s3 cp target/calcwebapp.war s3://mayur-s3-bucket-amazon-123456/ --recursive'
            }
        }
    }
}
