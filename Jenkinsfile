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
        sudo docker build -t kangdaeyoung/jenkinstest:pipetest .
        sudo docker push kangdaeyoung/jenkinstest:pipetest
        '''
      }
    }
    stage('deploy and service') {
      steps {
        sh '''
        ansible master -m shell -a 'sudo kubectl create deploy web-red --replicas=3 --port=80 --image=kangdaeyoung/jenkinstest:pipetest'
        ansible master -m shell -a 'sudo kubectl expose deploy web-red --type=LoadBalancer --port=80 --target-port=80 --name=web-red-svc'
        '''
      }
    }
  }
}
