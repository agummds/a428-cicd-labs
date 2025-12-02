pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
    }
}