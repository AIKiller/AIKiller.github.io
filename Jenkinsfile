pipeline {
  agent any
  stages {
    stage('fetch source code ') {
      steps {
        git(credentialsId: '183cd155-ae23-486e-bc99-27f58e720ae5', url: 'http://git.jiankangsn.com/root/hospital.git', branch: 'dev')
      }
    }
    stage('Test') {
      steps {
        sh 'git -version'
      }
    }
  }
}