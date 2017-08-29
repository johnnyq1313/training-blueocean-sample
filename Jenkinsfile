pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Back-End": {
            sh './jenkins/test-backend.sh'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "Front-End": {
            sh './jenkins/test-frontend.sh'
            junit(testResults: '**/test-results/karma/*.xml', allowEmptyResults: true)
            
          },
          "Static": {
            sh './jenkins/test-static.sh'
            
          },
          "Performance": {
            sh './jenkins/test-performance.sh'
            
          }
        )
      }
    }
    stage('Deploy to Staging') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }
    stage('Deploy to Production') {
      steps {
        input(message: 'Deploy to Production?', ok: 'Fire Away')
        sh './jenkins/deploy.sh production'
        sh 'echo Notifying appropriate team members!'
      }
    }
  }
}