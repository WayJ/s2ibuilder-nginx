pipeline {
  agent {
    node {
      label 'nodejs'
    }

  }

   environment {
        DOCKER_CREDENTIAL_ID = 'dockerhub-id'
        GITHUB_CREDENTIAL_ID = 'github-id'
        REGISTRY = 'harbor.wayj.online'
        IMAGE_NAMESPACE = 'tqmobiledev'
        GITHUB_ACCOUNT = 'WayJ'
        APP_NAME = 's2ibuilder-nginx-centos7-base'
    }

  stages {
    stage ('checkout scm') {
            steps {
                checkout(scm)
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
          sh 'make build .'
          withCredentials([usernamePassword(credentialsId : 'harborid' ,passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME' ,)]) {
            sh 'echo "$DOCKER_PASSWORD" | docker login $REGISTRY -u "$DOCKER_USERNAME" --password-stdin'
            sh 'docker push $REGISTRY/$IMAGE_NAMESPACE/$APP_NAME:$TAG_NAME'
          }

        }

      }
    }
  }
}