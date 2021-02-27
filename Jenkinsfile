pipeline {
		agent any
		// environment {
		//     PROJECT_ID = 'PROJECT-ID'
		//     CLUSTER_NAME = 'CLUSTER-NAME'
		//     LOCATION = 'CLUSTER-LOCATION'
		//     CREDENTIALS_ID = 'gke'
		// }
		stages {
				stage("Checkout code") {
						steps {
								checkout scm
						}
				}
				stage("Build image") {
						steps {
								script {
										myapp = docker.build("raniasaleh/hello:${env.BUILD_ID}")
								}
						}
				}
				stage("Push image") {
						steps {
								script {
										docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
														myapp.push("latest")
														myapp.push("${env.BUILD_ID}")
										}
								}
						}
				}
				stage('Deploy to ibmcloud') {
						steps{
							 sh '''
								 #!/bin/bash
								kubectl apply -f deployment.yaml -n test --validate=false
								'''
						}
				}
		}
}
