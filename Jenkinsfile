#!/usr/bin/env groovy 

pipeline {
    agent any

  tools {
        git 'Default'
    }

    stages {
        // stage('Git Clone') {
        //     steps {
        //         // Get some code from a GitHub repository
        //         git 'https://github.com/sudhanshu456/helm-charts-assignment.git'
        //     }
        // }


         stage('package') {
                steps{
                    sh 'wget https://get.helm.sh/helm-v3.6.1-linux-amd64.tar.gz'
                    sh 'ls -a'
                    sh 'tar -xvzf helm-v3.6.1-linux-amd64.tar.gz'
                    sh 'cp linux-amd64/helm /usr/bin'
                    sh 'helm version'
                    sh 'helm dependency update final-task'
                    sh 'helm lint final-task'
                    sh 'helm package final-task'
                    }
                post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    archiveArtifacts '*.tar.gz'
                }
            }
        }
        stage('Deploy') {
            steps{
                withCredentials([file(credentialsId: 'kindconf', variable: 'KUBECONFIG')]) {
                    sh 'cat $KUBECONFIG > ~/.kube/config'
                    sh 'helm install myrelease final-task'
                }
            }
        }

    }
}