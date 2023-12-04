pipeline {
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/JosephLeighton333/spring-petclinic.git'
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

    stage('Checkout') {
      steps {
        git(branch: 'main', credentialsId: 'GitHubID', url: 'https://github.com/JosephLeighton333/ansible.git')
      }
    }

    stage('Execute Ansible playbook') {
      steps {
        ansiblePlaybook 'copy_jar_file.yml'
      }
    }

    stage('Transfer jar file') {
      steps {
        script {
          def servers = ['vboxuser@10.0.2.4']
          servers.each { server ->
          sshPublisher(
            sshPublisherDesc: [
              configName: 'jenkinsSSHKey',
              hostname: "WebServer"

            ],
            publishers: [
              sshPublisherDesc(
                transfers: [
                  sshTransfer(
                    cleanRemote: false,
                    excludes: '',
                    execCommand: '',
                    flatten: false,
                    makeEmptyDirs: false,
                    noDefaultExcludes: false,
                    patternSeparator: '[, ]+',
                    remoteDirectory: '/home/vboxuser/',
                    remoteDirectorySDF: false,
                    remoteFiles: 'spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar',
                    removePrefix: '',
                    sourceFiles: 'spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar'
                  )
                ]
              )
            ]
          )
        }
      }

    }
  }

  stage('Deploy jar file') {
    steps {
      script {
        def servers = ['vboxuser@10.0.2.4']
        servers.each { server ->
        sshPublisher(
          sshPublisherDesc: [
            configName: 'jenkinsSSHKey',
            hostname: "WebServer"
          ],
          publishers: [
            sshPublisherDesc(
              transfers: [
                sshTransfer(
                  cleanRemote: false,
                  excludes: '',
                  execCommand: 'sudo systemctl stop spring-petclinic',
                  flatten: false,
                  makeEmptyDirs: false,
                  noDefaultExcludes: false,
                  patternSeparator: '[, ]+',
                  remoteDirectory: '/home/vboxuser/',
                  remoteDirectorySDF: false,
                  remoteFiles: '',
                  removePrefix: '',
                  sourceFiles: ''
                ),
                sshTransfer(
                  cleanRemote: false,
                  excludes: '',
                  execCommand: 'sudo cp /home/vboxuser/spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar /opt/your_service_directory/',
                  flatten: false,
                  makeEmptyDirs: false,
                  noDefaultExcludes: false,
                  patternSeparator: '[, ]+',
                  remoteDirectory: '/home/vboxuser/',
                  remoteDirectorySDF: false,
                  remoteFiles: '',
                  removePrefix: '',
                  sourceFiles: ''
                ),
                sshTransfer(
                  cleanRemote: false,
                  excludes: '',
                  execCommand: 'sudo systemctl start spring-petclinic',
                  flatten: false,
                  makeEmptyDirs: false,
                  noDefaultExcludes: false,
                  patternSeparator: '[, ]+',
                  remoteDirectory: '/home/vboxuser/',
                  remoteDirectorySDF: false,
                  remoteFiles: '',
                  removePrefix: '',
                  sourceFiles: ''
                )
              ]
            )
          ]
        )
      }
    }

  }
}

}
tools {
maven 'maven'
}
environment {
registry = 'josephleighton333/spring-petclinic'
registryCredential = 'docker-hub'
dockerImage = ''
}
}