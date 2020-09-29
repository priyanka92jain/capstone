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
            sh 'aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 004429287899.dkr.ecr.us-east-2.amazonaws.com'  
            sh 'docker build -t jainprg/my-app .'
            sh 'docker tag jainprg/my-app:latest 004429287899.dkr.ecr.us-east-2.amazonaws.com/capstone'
            sh 'docker push 004429287899.dkr.ecr.us-east-2.amazonaws.com/capstone'
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
