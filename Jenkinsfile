peline{
    agent{
        kubernetes{
           yaml"""
        apiVersion: v1
        kind: Pod
        metadata:
        spec:
          containers:
          - name: kubeval
            image: garethr/kubeval:latest
            imagePullPolicy: IfNotPresent
            command:
            - "watch"
            - "date"
            tty: true
        """
            }
        }
    
    stages {
        stage('Clone git') {
            steps{ 
                git url: 'https://github.com/FIXPETROVICH/Jenkins_kubeval.git', branch: 'master'
            }
        }    
        stage ('Validations') {
            parallel{
                stage ('validation Deploy_n.yaml'){    
                    steps {
                        container ('kubeval') { 
                            sh """#!/bin/sh
                            kubeval Deploy_n.yaml 
                            """
                        }
                    }
                }    
                stage ('validation Deploy_r.yaml'){    
                    steps {                 
                        container ('kubeval') {
                            sh """#!/bin/sh
                            kubeval Deploy_r.yaml 
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
