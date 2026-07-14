pipeline {
  agent any

  environment {
    AWS_REGION     = 'us-east-2'
    ECR_REGISTRY   = '386318011177.dkr.ecr.us-east-2.amazonaws.com'
    ECR_REPOSITORY = 'test'
    IMAGE_NAME     = 'test-flask'
    IMAGE_TAG      = 'latest'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('set up the venv') {
      steps {
        sh '''
          python3 -m venv venv
          ./venv/bin/pip install --upgrade pip
          ./venv/bin/pip install -r requirements.txt
        '''
      }
    }

    stage('RUN THE TESTS') {
      steps {
        script {
          int testStatus = sh(script: 'PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 ./venv/bin/pytest -q', returnStatus: true)
          if (testStatus != 0 && testStatus != 5) {
            error "Tests failed with exit code ${testStatus}"
          }
          if (testStatus == 5) {
            echo 'No tests were found; continuing.'
          }
        }
      }
    }

    stage('Login to ECR') {
      steps {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
          sh '''
            aws ecr get-login-password --region "$AWS_REGION" | \
            docker login --username AWS --password-stdin "$ECR_REGISTRY"
          '''
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          docker build -t "$IMAGE_NAME:$IMAGE_TAG" .
          docker tag "$IMAGE_NAME:$IMAGE_TAG" "$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG"
        '''
      }
    }

    stage('Push to ECR') {
      steps {
        sh 'docker push "$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG"'
      }
    }
  }
}