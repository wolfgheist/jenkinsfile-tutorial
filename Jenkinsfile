pipeline {
  agent any

  stages {
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