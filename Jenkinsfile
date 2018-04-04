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
        sh '''/usr/local/maven/bin/mvn -v 
/usr/local/maven/bin/mvn clean
chmod +x mvnw
./mvnw package -Pprod,swagger,zipkin docker:build -s /usr/local/maven/conf/settings.xml  -Dmaven.test.skip=true'''
      }
    }
    stage('deploy') {
      steps {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'rancher_service_node ', sshCredentials: [encryptedPassphrase: '{AQAAABAAAAAQ5+4ijIjviUKZkS5xGNnspsha4Ue97jmepbEhME5ZOcg=}', key: '', keyPath: '', username: 'root'], transfers: [sshTransfer(excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/usr/local/jenkins', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
  }
  environment {
    JAVA_HOME = '/usr/local/java'
    MVN_URL = 'https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.zip'
  }
}
