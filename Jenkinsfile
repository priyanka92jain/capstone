pipeline {
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
      }
    }
      stage('Push image') {
        steps {
          withAWS(region:'us-west-2',credentials:'aws-static') {
            sh 'aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 741383253344.dkr.ecr.us-west-2.amazonaws.com'  
            sh 'docker build -t priyanka/my-app .'
            sh 'docker tag priyanka/my-app:latest 741383253344.dkr.ecr.us-west-2.amazonaws.com/priyanka/my-app:latest'
            sh 'docker push 741383253344.dkr.ecr.us-west-2.amazonaws.com/priyanka/my-app:latest'
           }
        }
      }
      stage('Create kubeconfig') {
        steps {
          withAWS(region:'us-west-2',credentials:'aws-static') {
            sh 'aws eks --region us-west-2 update-kubeconfig --name priyanka-cluster'  
           }
        }
      }
      stage('Deploy containers') {
        steps {
          withAWS(region:'us-west-2',credentials:'aws-static') {
            sh 'kubectl apply -f deployment.yaml'  
            sh 'kubectl apply -f services.yaml'
           }
        }
      }       
  }
}  
