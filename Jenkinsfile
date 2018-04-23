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

docker tag hospital ${REGISTRY_HOST}/jhipster/hospital:${BRANCH_NAME}_${BUILD_ID}_hospital
docker tag hospital ${REGISTRY_HOST}/jhipster/hospital:${BRANCH_NAME}

export IMAGEID=$(docker images | grep hospital  | awk \'{print $3}\'|sort|uniq)

 if [ -n "$IMAGEID" ];
 then
    docker rmi -f $IMAGEID;
 fi
'''
      }
    }
    stage('deploy') {
      environment {
        RANCHER_ACCESS_KEY = '74443F7F23CA8394BC8D'
        RANCHER_SECRET_KEY = 'NtgrW94qjr4Yu9snQFXkAsMdsmVBHVpptrNWgu9h'
        RANCHER_HOST = 'http://172.60.30.51:8080'
        RANCHER_ENV = 'default'
        RANCHER_STACK = 'jhipster'
        RANCHER_ENV_URL = 'http://172.60.30.51:8080/env/1a5/apps/stacks/1st11/services/1s37/containers'
        PRIVATE_TOKEN = 'qGUM7oA3jm5ws3D-69zw'
        CI_COMMIT_REF_NAME = 'master'
        CI_PROJECT_NAME = 'hospital'
      }
      steps {
        sh '''export PATH=/usr/local/rancher:/usr/local/rancher-compose:$PATH

export IMAGEID=${REGISTRY_HOST}/jhipster/hospital:${BRANCH_NAME}_${BUILD_ID}_hospital

wget -q -N -P /tmp/${RANCHER_STACK}_${BUILD_NUMBER} --header "PRIVATE-TOKEN:${PRIVATE_TOKEN}" http://git.jiankangsn.com/root/docker-compose/raw/master/jhipster/templates/${CI_COMMIT_REF_NAME}/jhipster-rancher.yml http://git.jiankangsn.com/root/docker-compose/raw/master/jhipster/templates/${CI_COMMIT_REF_NAME}/jhipster-docker.yml

sed -i "s#\\.${CI_PROJECT_NAME}#${IMAGEID}#g;s#\\.git_hash#${BUILD_NUMBER}#g;s#${CI_PROJECT_NAME}-app#${CI_PROJECT_NAME}-${BUILD_NUMBER}-app#g" /tmp/${RANCHER_STACK}_${BUILD_NUMBER}/jhipster-docker.yml

sed -i "s#${CI_PROJECT_NAME}-app#${CI_PROJECT_NAME}-${BUILD_NUMBER}-app#g" /tmp/${RANCHER_STACK}_${BUILD_NUMBER}/jhipster-rancher.yml


rancher --url ${RANCHER_HOST}  --access-key ${RANCHER_ACCESS_KEY}  --secret-key ${RANCHER_SECRET_KEY}  --env ${RANCHER_ENV}  --wait-state healthy  up   --stack ${RANCHER_STACK}  --force-upgrade  --pull   --interval 10000  --file /tmp/${RANCHER_STACK}_${BUILD_NUMBER}/jhipster-docker.yml  --rancher-file /tmp/${RANCHER_STACK}_${BUILD_NUMBER}/jhipster-rancher.yml -d  ${CI_PROJECT_NAME}-${BUILD_NUMBER}-app '''
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