@Library('dynatrace@master') _

pipeline {
  agent {
    label 'maven'
  }
  environment {
    APP_NAME = "bookinfo-details"
    VERSION = readFile('version').trim()
    DOCKER_REPO = "${env.DOCKER_REGISTRY_URL}/${env.APP_NAME}"
    TAG = "${env.VERSION}"
  }
  stages {
    stage('Docker build') {
      steps {
        container('docker') {
          sh "cd src/review-wlpcfg && docker build -t ${env.DOCKER_REPO} ."
          sh "docker tag ${env.DOCKER_REPO} ${env.DOCKER_REPO}:${env.TAG}"
        }
      }
    }
    stage('Docker push to registry'){
      steps {
        container('docker') {
          withDockerRegistry([ credentialsId: "registry-creds", url: "" ]) {
            sh "docker push ${env.DOCKER_REPO}:${env.TAG}"
          }
        }
      }
    }
  }
}