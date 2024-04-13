pipeline {
	agent {label 'Jenkins_Agent'}
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
	stage("checkout from SCM"){
	        steps {
	git branch: 'main', credentialsId: 'github', url: 'https://github.com/rukmini-jadhav/register-app'
	      }
           }
	stage("build application") {
		steps {
			sh 'mvn clean package'
	       }
            }
	}
}
