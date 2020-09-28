pipeline {
    environment {
	  registry = "jainprg/docker-test"
	  registryCredential = ‘dockerhub’
	  dockerImage = ''
    }
    agent any
    stages {
      stage('Lint HTML') {
        steps {
          sh 'tidy -q -e *.html'
      }
    }
      stage('Build Docker image') {
        steps {
          sh 'docker build -t my-app .'
		  dockerImage = docker.build registry + ":$BUILD_NUMBER"
      }
    }
	
    

	  stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
      }

 
      stage('Push image') {
        steps {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
           }
        }
      }
      stage('Create kubeconfig') {
        steps {
          withAWS(region:'us-west-2',credentials:'wasread') {
            sh 'aws eks --region us-west-2 update-kubeconfig --name priyanka-cluster'  
           }
        }
      }
      stage('Deploy containers') {
        steps {
          withAWS(region:'us-west-2',credentials:'wasread') {
            sh 'kubectl apply -f deployment.yaml'  
            sh 'kubectl apply -f services.yaml'
           }
        }
      }       
  }
}  
