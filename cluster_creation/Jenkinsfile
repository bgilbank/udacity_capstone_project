pipeline {
    agent any

    stages {
        stage('Create Kubernetes Cluster') {
          steps {
           sh '''
              eksctl create cluster \
                --name cloudcapstone \
                --version 1.14 \
                --nodegroup-name standard-workers \
                --node-type t2.micro \
                --nodes 2 \
                --nodes-min 1 \
                --nodes-max 2 \
                --node-ami auto
            '''
           }
        }      
        stage('Create Configuration File') {
          steps {
            sh '''
              aws eks \
                --region us-west-2 \
                update-kubeconfig \
                --name cloudcapstone
            '''
          }
        }
      }
    }
