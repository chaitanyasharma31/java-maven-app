#!/usr/bin/env groovy
@Library('jenkins-shared-library')

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
            when {
                expression {
                    BRANCH_NAME =='master'
                }
            }
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    buildImage()
                   }
                }
            }
        stage('deploy') {
                        when {
                expression {
                    BRANCH_NAME =='master'
                }
                        }
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}
