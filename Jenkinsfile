pipeline {
  agent any

  stages {

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
        sh 'docker images --filter=reference="mintpay-svc:5"'
      }
    }
  }

  post {
    failure {
      echo 'Pipeline failed — please check the console log.'
    }
  }
}
