pipeline {
  agent {
    node {
      label 'linux'
    }

  }
  stages {
    stage('pre-prepare') {
      parallel {
        stage('fetch source code ') {
          steps {
            git(credentialsId: 'd903f52f-a5f3-4e42-9212-158ebf0069fe', url: 'http://git.jiankangsn.com/root/hospital.git', branch: 'dev')
          }
        }
        stage('login docker registry') {
          steps {
            sh 'sudo docker login -u ${REGISTRY_LOGIN} -p ${REGISTRY_PASSWORD} ${REGISTRY_HOST}'
          }
        }
      }
    }
    stage('builder') {
      steps {
        sh '''/usr/local/maven/bin/mvn -v 
/usr/local/maven/bin/mvn clean
chmod +x mvnw
./mvnw package -Pprod,swagger,zipkin docker:build -s /usr/local/maven/conf/settings.xml  -Dmaven.test.skip=true

docker tag hospital ${REGISTRY_HOST}/STACK/hospital:${BRANCH_NAME}_${CHANGE_TITLE}_${CHANGE_ID}
docker tag hospital ${REGISTRY_HOST}/jhipster/hospital:${CHANGE_TITLE}

export IMAGEID=$(docker images | grep hospital  | awk \'{print $3}\'|sort|uniq)

 if [ -n "$IMAGEID" ];
 then
    docker rmi -f $IMAGEID;
 fi
'''
      }
    }
  }
  environment {
    JAVA_HOME = '/usr/local/java'
    MVN_URL = 'https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.zip'
    REGISTRY_LOGIN = 'sndzjs@126.com'
    REGISTRY_PASSWORD = 'WUAcCepAdenyeb0'
    REGISTRY_HOST = 'registry.cn-qingdao.aliyuncs.com'
  }
}