pipeline{
    //agent any
    agent {label 'jenkins-agent'}
    options{
        buildDiscarder(logRotator(daysToKeepStr: '15'))
        disableConcurrentBuilds()
        timeout(time: 5, unit: 'MINUTES')
        retry (3)
    }
    parameters{
        string(name: 'BRANCH', defaultValue: 'main')
        string(name: 'BUILD_NUMBER', defaultValue: '1')
    }
    stages{
       stage('Git checkout'){
        steps{
            checkout scm
        }
       } 
       stage('Build and Push'){
        steps{
            sh '''
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 049721949876.dkr.ecr.us-east-1.amazonaws.com
                cd vote && docker build -t 049721949876.dkr.ecr.us-east-1.amazonaws.com/ci_jenkins:v${BUILD_NUMBER} .
                docker push 049721949876.dkr.ecr.us-east-1.amazonaws.com/ci_jenkins:v${BUILD_NUMBER}
            '''
        }
       }
    }
    post{
        always{
            sh "echo running final steps"
        }
    }

}
