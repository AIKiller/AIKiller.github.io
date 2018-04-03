pipeline {
  agent any
  stages {
    stage('fetch source code ') {
      steps {
        git(credentialsId: 'd903f52f-a5f3-4e42-9212-158ebf0069fe', url: 'http://git.jiankangsn.com/root/hospital.git', branch: 'dev')
      }
    }
    stage('bulider') {
      steps {
        sh '''docker ps
chmod +x mvnw
./mvnw package -Pprod,swagger,zipkin docker:build -s /usr/local/maven/conf/settings.xml  -Dmaven.test.skip=true -Dmaven.test.failure.ignore=true'''
      }
    }
    stage('deploy') {
      steps {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'rancher_service_node ', transfers: [sshTransfer(excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/usr/local/jenkins', remoteDirectorySDF: false, removePrefix: '', sourceFiles: ' /target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
  }
}