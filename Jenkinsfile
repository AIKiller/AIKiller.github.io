pipeline {
  agent any
  stages {
    stage('fetch source code ') {
      steps {
        git(credentialsId: 'd903f52f-a5f3-4e42-9212-158ebf0069fe', url: 'http://git.jiankangsn.com/root/hospital.git', branch: 'dev')
      }
    }
    stage('Test') {
      steps {
        sh 'git -version'
      }
    }
  }
}