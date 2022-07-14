#!/usr/bin/env groovy 

pipeline {
    agent any

  tools {
        git 'Default'
    }

    stages {

         stage('package') {
                steps{
                    sh 'helm version'
                    sh 'helm dependency update final-task'
                    sh 'helm lint final-task'
                    sh 'helm package final-task'
                    }
                post {
                success {
                    archiveArtifacts '*.tgz'
                }
            }
        }
        stage('Deploy') {
            steps{
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh 'mkdir -p ~/.kube'
                    sh 'cat $KUBECONFIG > ~/.kube/config'
                    sh 'helm upgrade myrelease final-task-0.1.0.tgz'
                }
            }
        }
        

    }
}