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
        sh '''export MAVEN_HOME=\'/usr/local/maven\'
mvn -v 
mvn clean
echo "distributionUrl=${MVN_URL}" > `pwd`/.mvn/wrapper/maven-wrapper.properties
echo $PATH
chmod +x mvnw
./mvnw package -Pprod,swagger,zipkin docker:build -s /usr/local/maven/conf/settings.xml  -Dmaven.test.skip=true'''
      }
    }
  }
  environment {
    JAVA_HOME = '/usr/local/java'
    MVN_URL = 'https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.zip'
  }
}