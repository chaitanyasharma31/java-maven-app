#!/usr/bin/env groovy
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
                    gv.buildJar()
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    gv.buildImage()
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
