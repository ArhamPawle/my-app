@Library("app-lib") _
pipeline {
  agent any

  tools {
    maven 'maven3'
  }
  options {
    buildDiscarder logRotator(daysToKeepStr: '10', numToKeepStr: '7')
  }
  parameters {
    choice choices: ['develop', 'qa', 'master'], description: 'Choose the branch to build', name: 'branchName'
  }
  stages {
    stage('Maven Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    
    stage('Initialize'){
        def dockerHome = tool 'arhampawle'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
    stage('Deploy to Tomcat') {
      steps {
        tomcatDeploy(["172.31.13.38","172.31.13.38","172.31.13.38"],"ec2-user","tomcat-dev")
      }
    }
  }
  post {
    success {
      archiveArtifacts artifacts: 'target/*.war'
      cleanWs()
    }
  }
}
