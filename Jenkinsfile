pipeline {
  agent { label 'test3' }
    yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: test3
    image: garethr/kubeval:latest
    command:
      - cat
    tty: true
"""
    }
  }
  triggers { pollSCM 'H/15 * * * *' }
  stages {
    stage('Clone repository') { 
      steps {
        git url: 'https://github.com/FIXPETROVICH/jenkins_kubeval.git', branch: 'master' 
      }
    }
    stage('Kubeval manifests') {
      parallel {
        stage('Check node-exporter.yaml') {
          steps {
            container('test3') {
              sh """#!/bin/sh
                    kubeval manifests/node-exporter.yaml  
                 """
            }
          }
        }
        stage('Check ms.yaml') {
          steps {
            container('test3') {
              sh """#!/bin/sh
                    kubeval manifests/ms.yaml
                 """
            }
          }
        }         
      }
    }
  }
  post {
    success {
      slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
    failure {
      slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
  }
           }
