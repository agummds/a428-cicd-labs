pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
        }
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'react-app', url: 'https://github.com/agummds/a428-cicd-labs.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh '''
                  cd react-app

                  if [ -f "./jenkins/scripts/test.sh" ]; then
                    chmod +x ./jenkins/scripts/test.sh
                    ./jenkins/scripts/test.sh
                  else
                    echo "No test script found, skipping..."
                  fi
                '''
            }
        }

        stage('Archive artifacts') {
            steps {
                archiveArtifacts artifacts: 'react-app/build/**', fingerprint: true
            }
        }

    }
}
