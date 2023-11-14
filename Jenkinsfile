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

    stage('execute') {
      steps {
        dir(path: '/var/lib/jenkins/workspace/jenkins-script_main/target') {
          bat 'java -jar spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar'
        }

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