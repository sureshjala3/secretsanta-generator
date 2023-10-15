pipeline {
    agent any
    // tools{
    //     jdk 'jdk17'
    //     maven 'maven3'
    // }
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }

    stages {
        stage('git-checkout') {
            steps {
                git 'https://github.com/sureshjala3/secretsanta-generator.git'
            }
        }

        stage('Code-Compile') {
            steps {
               sh "mvn clean compile"
            }
        }
        
        stage('Unit Tests') {
            steps {
               sh "mvn test"
            }
        }
        
		// stage('OWASP Dependency Check') {
  //           steps {
  //              dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC'
  //                   dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
  //           }
  //       }


        stage('Sonar Analysis') {
            steps {
               withSonarQubeEnv('sonar'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Santa \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Santa '''
               }
            }
        }

		 
        stage('Code-Build') {
            steps {
               sh "mvn clean package"
            }
        }

         stage('Docker Build') {
            steps {
                    sh '''
		     aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 497511884140.dkr.ecr.us-east-1.amazonaws.com
		     docker build -t  santa123:latest .
                     docker tag  santa123:latest 497511884140.dkr.ecr.us-east-1.amazonaws.com/my-test-repo:latest
		     docker push 497511884140.dkr.ecr.us-east-1.amazonaws.com/my-test-repo:latest
      		'''
            }
        }        
        	 
        // stage('Docker Image Scan') {
        //     steps {
        //        sh "trivy image adijaiswal/santa123:latest "
        //     }
        // }}
        
         // post {
         //    always {
         //        emailext (
         //            subject: "Pipeline Status: ${BUILD_NUMBER}",
         //            body: '''<html>
         //                        <body>
         //                            <p>Build Status: ${BUILD_STATUS}</p>
         //                            <p>Build Number: ${BUILD_NUMBER}</p>
         //                            <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
         //                        </body>
         //                    </html>''',
         //            to: 'jaiswaladi246@gmail.com',
         //            from: 'jenkins@example.com',
         //            replyTo: 'jenkins@example.com',
         //            mimeType: 'text/html'
         //        )
         //    }
        }
		
		

    
}
