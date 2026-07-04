pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ghcr.io/wolfgheist/jenkinsfile-tutorial'
    }

    stages {
        stage('Build Docker image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Push to GHCR') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_TOKEN')]) {
                    sh '''
                    echo "$GITHUB_TOKEN" | docker login ghcr.io -u "$GITHUB_USER" --password-stdin
                    docker push ${IMAGE_NAME}:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            sh 'echo "build successful"'
        }
        failure {
            sh 'echo "build failed"'
        }
    }
}
