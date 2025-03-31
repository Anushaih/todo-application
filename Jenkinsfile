pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
 	DOCKER_IMAGE = 'anushaheralagi/todo-application-image'
	DOCKER_TAG = 'latest'       
    }
    stages {
        stage('Checkout Code'){
            steps {
                git branch: 'main', url: 'https://github.com/Anushaih/todo-application.git'
                
            }
        }
	stage('Build doc image') {
	    steps{
		sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
	    }
        }
	stage('pusj image to doc hub') {
	   steps {
	    script {
      		docker.withRegistry('', 'docker-hub-credentials') {
		   docker.image("$DOCKER_IMAGE:$DOCKER_TAG").push()
     	        }
	    }
	   }
        }
	stage('deploy with kuber') {
  	    steps{
		sh 'kubectl apply -f todo-application-deployment.yaml'
		sh 'kubectl expose deployment todo-application --type=NodePort --port=8081'
	    }
	}
	stage('Test minikube app') {
            steps {
  		sh 'curl http://$(minikube ip):30080'
	    }
        }
    }
    post {
	    success {
		echo 'success'
	    }
	    failure {
   		echo 'fail'
	    } 
    }
}
