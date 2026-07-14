pipeline {
  agent any

  environment {
    VENV = 'venv'
  }

  stages {
    stage('set up the venv') {
      steps {
        sh 'python3 -m venv venv'
        sh '. venv/bin/activate'
        sh 'python3 -m pip install --upgrade pip'
        sh 'python3 -m pip install -r requirements.txt'
      }
    }

    stage('RUN THE TESTS') {
      steps {
        sh 'python3 -m unittest discover -s tests'
      }
    }
  }
}