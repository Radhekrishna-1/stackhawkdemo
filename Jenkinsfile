pipeline {
  //agent { label 'linux' }
  agent any
  stages {
    stage("Checkout code") {
      steps {
        checkout scm
      }
    }
    stage("Deploy site") {
      steps {
        sh 'pwd'
        sh 'sudo cp index.html /var/www/html'
      }
    }
    stage("Pull HawkScan Image") {
      steps {
        sh 'docker pull stackhawk/hawkscan'
        sh 'pwd'
        sh 'docker images'
      }
    }
    stage("Run HawkScan Test") {
      environment {
        HAWK_API_KEY = credentials("stackhawk-api-key")
      }
      steps {
        sh "docker run -v ${WORKSPACE}:/hawk:rw -t \
            -e API_KEY=${env.HAWK_API_KEY} \
            -e NO_COLOR=true \
            stackhawk/hawkscan"
      }
    }
  }
}
