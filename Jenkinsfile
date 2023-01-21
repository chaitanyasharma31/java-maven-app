#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://github.com/chaitanyasharma31/jenkins-shared-library.git',
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
                    buildImage 'cshrma/java-maven-app'
                    dockerLogin()
                    dockerPush 'cshrma/java-maven-app'
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
