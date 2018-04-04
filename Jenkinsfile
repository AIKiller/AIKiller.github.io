pipeline {
  agent {
    node {
      label 'linux'
    }
    
  }
  stages {
    stage('fetch source code ') {
      steps {
        git(credentialsId: 'd903f52f-a5f3-4e42-9212-158ebf0069fe', url: 'http://git.jiankangsn.com/root/hospital.git', branch: 'dev')
      }
    }
    stage('builder') {
      steps {
        sh '''echo $PATH
chmod +x mvnw
./mvnw package -Pprod,swagger,zipkin docker:build -s /usr/local/maven/conf/settings.xml  -Dmaven.test.skip=true'''
      }
    }
  }
  environment {
    JAVA_HOME = '/usr/local/java'
  }
}