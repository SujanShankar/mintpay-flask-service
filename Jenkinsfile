pipeline {
  agent any
  options {
    skipDefaultCheckout(true)
  }

  stages {

    stage('Checkout') {
      steps {
        // FULL CLEAN CHECKOUT
        deleteDir()
        checkout([$class: 'GitSCM',
          branches: [[name: '*/main']],
          userRemoteConfigs: [[url: 'https://github.com/SujanShankar/mintpay-flask-service.git']]
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
      }
    }
  }

  post {
    failure {
      echo 'Pipeline failed — please check the console log.'
    }
  }
}
