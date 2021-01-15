pipeline {
  agent {
    docker {
      label 'dockerhost-label'
      image 'python:3.8-buster'
      args '-u root'
    }
  }
  stages {

    stage ('Prepare') {
    
      steps {
        sh '''
          pip install pytest-testinfra pytest-html paramiko
        '''
      }
    }

    stage ('Test') {
      steps {
          sshagent(credentials : ['jenkins-ssh']) {
            sh '''
              pytest --html=report.html --self-contained-html --disable-warnings --hosts=lab --ssh-config=./ssh_config test_*.py
            '''
          }
          archiveArtifacts "report.html"
      }
    }

  } 

  post { 
    always { 
      cleanWs()
    }
  }

}
