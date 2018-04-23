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
        rancher(service: 'test/hospital', image: 'hospital', confirm: true, environmentId: '1a5', endpoint: 'http://172.60.30.51:8080/v2-beta', credentialId: '863ce475-8862-4efa-9e01-813ba36e095b', timeout: 10, ports: '80', environments: 'tes')
      }
    }
  }
  environment {
    JAVA_HOME = '/usr/local/java'
    MVN_URL = 'https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.zip'
  }
}