#!/usr/bin/env groovy
library identifier: 'jenkins-shared-library@master', retriver: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://github.com/chaitanyasharma31/jenkins-shared-library.git'
         credentials: 'github-credentials'
         ]
)

def gv
pipeline {
    agent any
    tools {
        maven 'maven-3.8.7'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                    echo "Executing pipeline for branch $BRANCH_NAME"
                }
            }
        }
        stage('build jar') {

            steps {
                script {
                    buildJar()
                }
            }
        }
        stage('build and push image') {
            steps {
                script {
                    buildImage 'cshrma/demo-app:jma-3.0'
                    dockerLogin()
                    dockerPush 'cshrma/demo-app:jma-3.0'
                   }
                }
            }
        stage('deploy') {
            steps {
                script {
                    gv.deployApp()
                                           
                }
            }
        }
    }
}
