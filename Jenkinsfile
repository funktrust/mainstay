pipeline {
    environment {
    registryCredential = 'funktrust-dockerhub'
  }
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            some-label: some-label-value
        spec:
            containers:
            - name: docker
                image: docker:latest
                command:
                - cat
                tty: true
            volumes:
            - hostPathVolume:
                mountPath: '/var/run/docker.sock'
                hostPath: '/var/run/docker.sock'
        '''
    }
  }
  stages {
    stage('Clone repository') {
      steps {
        git url:'https://github.com/funktrust/mainstay.git', branch:'master'
      }
    }

    stage('Run docker') {
      steps {
        container('docker') {
          sh 'docker build -t mainstay:latest .'
        }
      }
    }
  }
}