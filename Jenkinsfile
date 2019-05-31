#!/usr/bin/env groovy
pipeline {
  agent any
  options { skipDefaultCheckout()
            disableConcurrentBuilds()
          }

  stages {
    stage('Build Dependencies') {
      steps {
        checkout scm
        sh "npm install"
      }
    }
     stage('Package') {
        steps {
         sh  "npm run build-aws-resource"
         sh "aws s3 ls"
         sh "aws s3 cp cloudexample.zip s3://stevenmcbucket/cloudexample.zip"
         sh "aws s3 cp cloudformation.template s3://stevenmcbucket/cloudformation.template"
        }
    }
     stage('Build') {
         steps {
          sh 'aws cloudformation create-stack --template-url https://stevenmcbucket.s3.amazonaws.com/cloudformation.template --stack-name steventeststack --capabilities CAPABILITY_IAM --region us-east-1\n'
          sh 'aws cloudformation wait stack-create-complete --stack-name steventeststack --region us-east-1'
         }
     }
    }
}