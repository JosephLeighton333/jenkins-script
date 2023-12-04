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
        git(branch: 'main', credentialsId: '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', url: 'https://github.com/JosephLeighton333/ansible.git')
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
              configName: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC9sUdZBe6cvs5/IFXBBxRR/WLZhwOy5X/NuQWQwmqVeglts01hzNm1JcEY08LjWNQtrPy8OBGOZbY9mTK06LD4ReI4CMC3kVrt9YE7+f+F1RvTByYp8eFrKSTirDX1LNrsLFdAyoWRl8LsDdrebY1Rh+T3iv9g9hnhbdGygaBt1w5KdoEmUE1+S9UjGl0s/ux7hJ5RK1EgyuLSMfQyzmkSL4MLwFpUNIwGq0ZkkPSrkX2VFmqcNiUdlUfeSn6HJffRuqu4rBFqxWSL82xvkq4cwrYC5leirbw3r/gj24OSvcrsq+Atxc5jBPMXk0bxE8fjrgBWWdgzRu1P7cRsA6itpsW/bHP566G+o1XfC+vS0N6SW49WHcMLU6FJUp7LzS3YA8TTvZHEwGHuexSYw1dLXLhw/JB6WcZxoxCFveVy8jLweOzxF8Efh95k0EVxn3m5DFSjtq8VZyPlMj8I6qt6sgwh+//erd4GkWg72AtrZLqzw81qUgU5V31T9qYmXlk= vboxuser@WebServer',
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
            configName: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC9sUdZBe6cvs5/IFXBBxRR/WLZhwOy5X/NuQWQwmqVeglts01hzNm1JcEY08LjWNQtrPy8OBGOZbY9mTK06LD4ReI4CMC3kVrt9YE7+f+F1RvTByYp8eFrKSTirDX1LNrsLFdAyoWRl8LsDdrebY1Rh+T3iv9g9hnhbdGygaBt1w5KdoEmUE1+S9UjGl0s/ux7hJ5RK1EgyuLSMfQyzmkSL4MLwFpUNIwGq0ZkkPSrkX2VFmqcNiUdlUfeSn6HJffRuqu4rBFqxWSL82xvkq4cwrYC5leirbw3r/gj24OSvcrsq+Atxc5jBPMXk0bxE8fjrgBWWdgzRu1P7cRsA6itpsW/bHP566G+o1XfC+vS0N6SW49WHcMLU6FJUp7LzS3YA8TTvZHEwGHuexSYw1dLXLhw/JB6WcZxoxCFveVy8jLweOzxF8Efh95k0EVxn3m5DFSjtq8VZyPlMj8I6qt6sgwh+//erd4GkWg72AtrZLqzw81qUgU5V31T9qYmXlk= vboxuser@WebServer',
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