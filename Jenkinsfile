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
            sh '''
              PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 ./venv/bin/pytest -q
              status=$?
              if [ "$status" -ne 0 ] && [ "$status" -ne 5 ]; then
                exit "$status"
              fi
            '''
        }
    }
  }
}