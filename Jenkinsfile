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
	 }
}
