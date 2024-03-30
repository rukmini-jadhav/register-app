pipeline {
	agent {label 'jenkins_agent'}
	tools {
		jdk 'java17'
		maven 'Maven3'
	}
	stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
	}
	stage("Checkout from SCM"){
         steps {
          git branch: 'main', credentialsId: 'github', url: 'https://github.com/rukmini-jadhav/register-app.git'
                }
        }
	  stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
         }
       }
   }
