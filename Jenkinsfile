pipeline {   
    agent {
        label 'ag-2' 
    }
	    environment {
        IMAGE_NAME = "calcwebappmvn:${BUILD_NUMBER}"
    }

      tools {
         maven 'xyz-maven'
     //    dockerTool 'my-docker'
      }
    stages {
        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/mayur-z/calcwebappmvn.git'    
		            echo "Code Checked-out Successfully!!";
		            sh 'ls -la'
            }
        }
		stage('SonarQube analysis') {
            steps {
		// Change this as per your Jenkins Configuration
                withSonarQubeEnv('sonar-tok') {
                    sh 'mvn package sonar:sonar'
                }
            }
        }
		
    //         stage('Package') {
    //             steps {
				// 	sh 'ls -la'
				// 	sh 'mvn clean'
    //                 sh 'mvn package'    
    // 		            echo "Maven Package Goal Executed Successfully!";
    // 		            sh  'ls -la'
    //             }
    //         }
    //     stage('docker') {
    //         steps {
				// sh 'which docker'
				// sh 'docker --version'
				// sh 'docker ps'
    //             sh 'docker images'
    //            // sh 'docker build -t calcwebappmvn:v1 .' 
    //             sh 'docker build -t ${IMAGE_NAME} .'
    //             echo "Docker Image Built Successfully!!"
    //             sh 'docker images'
    //                // junit 'target/surefire-reports/*.xml'
		  //          //     echo "Publishing JUnit reports"
    //         }
    //     }
//         stage('Jacoco Reports') {
//             steps {
//                   jacoco()
//                   echo "Publishing Jacoco Code Coverage Reports";
//             }
//         }

// // 	stage('SonarQube analysis') {
//             steps {
// 		// Change this as per your Jenkins Configuration
//                 withSonarQubeEnv('SonarQube') {
//                     bat 'mvn package sonar:sonar'
//                 }
//             }
//         }
    }
    post {
        success {
            echo 'pipeline is successful'
        }
        failure {
            echo 'pipeline is FAILED'
        }
    }
}
