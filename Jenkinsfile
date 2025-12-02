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
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}
