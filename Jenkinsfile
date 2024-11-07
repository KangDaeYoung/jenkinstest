pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/KangDaeYoung/jenkinstest', branch: 'main'
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        sudo docker build -t 211.183.3.199/harbortest:pipetest .
        sudo docker push 211.183.3.199/harbortest:pipetest
        '''
      }
    }
    stage('deploy and service') {
      steps {
        sh '''
        ansible master -m shell -a 'sudo kubectl create deploy web-red --replicas=3 --port=80 --image=211.183.3.199/harbortest:pipetest'
        ansible master -m shell -a 'sudo kubectl expose deploy web-red --type=LoadBalancer --port=80 --target-port=80 --name=web-red-svc'
        '''
      }
    }
  }
}
