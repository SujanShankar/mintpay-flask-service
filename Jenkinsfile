pipeline {
  agent any

  options {
    skipDefaultCheckout(true)   // IMPORTANT: prevents Jenkins auto-checkout
  }

  stages {

    stage('Checkout') {
      steps {
        script {
          deleteDir()  // FULL CLEAN before checkout
        }
        checkout([
          $class: 'GitSCM',
          branches: [[name: 'main']],   // CORRECT
          userRemoteConfigs: [[
            url: 'https://github.com/SujanShankar/mintpay-flask-service.git'
          ]]
        ])
      }
    }

    stage('Pre-check Docker') {
      steps {
        sh 'docker --version'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t mintpay-svc:5 .'
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker rm -f mintpay-svc || true'
        sh 'docker run -d --name mintpay-svc -p 12240:5000 mintpay-svc:5'
      }
    }

    stage('Verify') {
      steps {
        sh 'docker ps --filter name=mintpay-svc --format "{{.Names}} {{.Image}} {{.Ports}}"'
        sh 'curl -s http://host.docker.internal:12240 || true'
      }
    }
  }

  post {
    failure {
      echo 'Pipeline failed — please check logs.'
    }
  }
}
