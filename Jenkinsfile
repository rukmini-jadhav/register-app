pipeline {
	agent {label 'jenkins_Agent'}
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
         
         }
   }
