pipeline { 
    agent any
    environment { 
        registry = "photop/micro_focus" 
        registryCredential = 'dockerhub_id'
        dockerImage = ""
    } 
    stages {
        stage('properties') {
            steps {
                script {
                    properties([pipelineTriggers([pollSCM('*/30 * * * *')])])
                    properties([buildDiscarder(logRotator(daysToKeepStr: '5', numToKeepStr: '20')),])
                }
                git https://github.com/photop33/Testing_for_job.git'
            }
        }
                  stage('Flask.py') {
            steps {
                script {
                    bat 'start /min python Flask.py'
                    bat 'echo success Flask.py'
                }
            }
        }
        stage('Backend_testing') {
            steps {
                script {
                    bat 'start Backend_testing.py'
                    bat 'echo success Backend_testing'
                }
            }
        }
	stage('Fronted_testing') {
            steps {
                script {
                    bat 'start Fronted.py'
                    bat 'echo success Fronted_testing'
                }
            }
        }
	stage('clean_environemnt') {
            steps {
                script {
                    bat 'start/min python3 clean_environemnt.py'
                    bat 'echo success clean_environemnt-1'
                 }
            }
        }    
	stage('Build Docker image - locally') {
            steps {
                script{
                    bat "docker build -t \"$BUILD_NUMBER\" ."
                    bat "start/min docker run -p 8777:8777 $BUILD_NUMBER "
                }
            }
         }
	stage('build and push image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
		    bat 'echo success Build'
                    docker.withRegistry('', registryCredential) {	
                    dockerImage.push() 	
            }
         }     
       }	 
    }
  }
}
