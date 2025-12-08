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

        /* ======================================================
         * Kriteria 4 — Manual Approval Stage (HARUS pakai input)
         * ====================================================== */
        stage('Manual Approval') {
            steps {
                script {
                    input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
                }
            }
        }

        /* ======================================================
         * Kriteria 3 — Deploy + Jeda 1 Menit (HARUS pakai sleep)
         * ====================================================== */
        stage('Deploy') {
            steps {
                sh '''
                    echo "Building React App..."
                    npm run build

                    echo "Starting temporary server..."
                    npx serve -s build &
                    APP_PID=$!

                    echo "Keeping app running for 60 seconds..."
                    sleep 60

                    echo "Stopping the server..."
                    kill $APP_PID
                '''
            }
        }
    }
}
