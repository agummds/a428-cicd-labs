properties([
  pipelineTriggers([
    pollSCM('H/2 * * * *')   // ← INI POLL SCM-NYA
  ])
])

node {
  stage('Checkout') {
    checkout([$class: 'GitSCM',
      branches: [[name: '*/react-app']],
      userRemoteConfigs: [[url: 'https://github.com/agummds/a428-cicd-labs.git']]
    ])
  }

  stage('Build') {
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
      sh '''
        cd react-app
        npm install
        npm run build
      '''
    }
  }

  stage('Test') {
    docker.image('node:16-buster-slim').inside {
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
    archiveArtifacts artifacts: 'react-app/build/**', fingerprint: true
  }
}
