pipeline {
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/talitz/spring-petclinic-jenkins-pipeline.git'
      }
    }

    stage('Compile') {
      steps {
        sh 'mvn compile'
      }
    }

    stage('Test') {
      steps {
        sh '''
        mvn clean install
        ls
        pwd
        '''
      }
    }

  }
  tools {
    maven 'maven'
  }
  environment {
    registry = 'tyitzhak/spring-petclinic-hub'
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
}