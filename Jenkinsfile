#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'git@github.com:chaitanyasharma31/jenkins-shared-library.git',
     credentialsId: 'github-credentials'
    ]
)

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        IMAGE_NAME = 'cshrma/demo-app:1.0'
    }
    stages {
        stage('build app') {
            steps {
               script {
                  echo 'building application jar...'
                  buildJar()
               }
            }
        }
        stage('build image') {
            steps {
                script {
                   echo 'building docker image...'
                   buildImage(env.IMAGE_NAME)
                   dockerLogin()
                   dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                   echo 'deploying docker image to EC2...'

                //    def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                //    def ec2Instance = "ec2-user@99.79.77.236"

                //    sshagent(['ec2-server-key']) {
                //        sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
                //        sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                //        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                    def dockerCmd = "docker run -p 8080:8080 -d ${IMAGE_NAME}"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@99.79.77.236 ${dockerCmd}"
                   }
                }
            }
        }
    }
}
