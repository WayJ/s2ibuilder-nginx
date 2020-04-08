pipeline {
  agent {
    node {
      label 'nodejs'
    }

  }
  stages {
    stage('git clone') {
      steps {
        git(url: 'http://jiangwei@gitlab.hztianque.com/sd-mobile/common/TQPrismEnv.git', credentialsId: 'gitlabid', branch: 'master', changelog: true, poll: false)
      }
    }
    stage('env check') {
      steps {
        container('nodejs') {
          sh 'npm -v'
        }

      }
    }
    stage('builld & push') {
      steps {
        container('nodejs') {
          sh 'docker build -f Dockerfile -t $REGISTRY/tqmobile/tq-prism:SNAPSHOT-$BUILD_NUMBER .'
          withCredentials([usernamePassword(credentialsId : 'harborid' ,passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME' ,)]) {
            sh 'echo "$DOCKER_PASSWORD" | docker login $REGISTRY -u "$DOCKER_USERNAME" --password-stdin'
            sh 'docker push $REGISTRY/tqmobile/tq-prism:SNAPSHOT-$BUILD_NUMBER'
          }

        }

      }
    }
  }
}
