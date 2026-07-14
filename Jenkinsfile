pipeline {
  agent any

  environment {
    VENV = 'venv'
  }

  stages {
    stage('set up the venv') {
      steps {
        sh 'python3 -m venv $VENV'
        sh '. $VENV/bin/activate'
        sh 'python -m pip install --upgrade pip'
        sh 'pip install -r requirements.txt'
      }
    }

    stage('RUN THE TESTS') {
      steps {
        sh 'python -m unittest discover -s tests'
      }
    }
  }
}