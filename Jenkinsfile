pipeline {
	agent {label 'Jenkins_Agent'}
	tools {
		jdk 'java17'
		maven 'Maven3'
	}
	environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "rukminihub"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")

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
		stage("test application"){
			steps {
		    sh 'mvn test'
			}
		}
		stage("sonarqube analyasis"){
		steps {
		   script {
		withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
		   sh "mvn sonar:sonar"
	                     }
		         }
	           }
	       }
	        stage("quality gate"){
		  steps {
		     script {
		waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
				}
			}
		}
		stage("build & push docker image"){
		steps {
                      script {
                  docker.withRegistry('',DOCKER_PASS) {
                  docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')	
	                }
                    }
		}
		stage("Trigger CD Pipeline") {
            steps {
                script {
			sh "curl -v -k --user rukmini jadhav:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'ec2-13-231-228-22.ap-northeast-1.compute.amazonaws.com:8080/job/gitops-register-app-cd /buildWithParameters?token=gitops_token'"        
                }
            }
       }
    }
	post {
       failure {
             emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                      subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                      mimeType: 'text/html',to: "rukminijadhav7066@gmail.com"
      }
      success {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                     subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                     mimeType: 'text/html',to: "rukminijadhav7066@gmail.com"
                  }      

	      }
       }

