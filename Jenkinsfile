pipeline {
    agent any

    tools {
        maven 'maven1'
    }

    environment {
        my_aws_credential = credentials('aws-access')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/mayurpandit25/calcwebappmvn.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                ls -la
                mvn clean package
                ls -la target
                '''
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.war'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        try {
                            def qg = waitForQualityGate()
                            echo "Quality Gate Status: ${qg.status}"

                            if (qg.status != 'OK') {
                                error "Quality Gate failed: ${qg.status}"
                            }
                        } catch (Exception e) {
                            echo "Quality Gate check failed: ${e.message}"
                            error "Quality Gate stage failed"
                        }
                    }
                }
            }
        }

        stage('SonarQube Report') {
            steps {
                sh 'ls -la'
                echo 'SonarQube analysis completed'
            }
        }

        stage('Docker Build and Push') {
            steps {
                sh '''
                ls -la
                docker build -t calc:v1 .
                docker images
                '''
            }
        }

        stage('Docker push to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 632295789918.dkr.ecr.us-west-2.amazonaws.com
                docker tag calc:v1 632295789918.dkr.ecr.us-west-2.amazonaws.com/app_cal:latest
                docker push 632295789918.dkr.ecr.us-west-2.amazonaws.com/app_cal:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                aws eks update-kubeconfig --name my-cluster-mayur --region us-west-2
                kubectl apply -f calc-deployment-svc.yaml
                kubectl get all
                '''
            }
        }

        stage(Deployment Verification) {
            steps {
                sh '''
                kubectl get all 
                kubectl get pods
                sleep 10
                kubectl get svc -o wide
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline execution completed.'
        }
        error {
            echo 'Pipeline execution failed.'
        }
    }
}
