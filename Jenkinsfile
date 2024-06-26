@Library('shared-library')_
pipeline {
    agent {
	    node{
		    label 'jenkins-slave'
	    }
    }
    
    environment {
        dockerHubCredentialsID	    = 'DockerHub-cred'  		    			// DockerHub credentials ID.
        imageName   		    = 'ramy282/jenkins-lab1' 	    			// DockerHub repo/image name.
	openshiftCredentialsID	    = 'openshift-token'		         		// service account token credentials ID
	openshiftClusterURL	    = 'https://api.ocp-training.ivolve-test.com:6443'   // OpenShift Cluser URL.
        openshiftProject 	    = 'ramyanwar'			     		// OpenShift project name.

    }
     stages {
	
        stage('Build Docker Image') {
            steps {
                script {
                	// Navigate to the directory contains Dockerfile
                 		buildDockerImage("${imageName}")
                }
            }
        }
        stage('push Docker Image') {
            steps {
                script {
                	// Navigate to the directory contains Dockerfile
                 		pushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                }
            }
        }


        stage('Deploy on OpenShift Cluster') {
            steps {
                script { 
				deployToOpenShift("${openshiftCredentialsID}", "${openshiftClusterURL}", "${openshiftProject}", "${imageName}")
                }
            }
        }
	     
     }

    post {
        success {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline succeeded"
        }
        failure {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline failed"
        }
    }
}
