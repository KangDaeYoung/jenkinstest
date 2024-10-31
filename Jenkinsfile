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
        ansible master -m copy -a "src=jenkins.yml dest=/root/testpod.yml" --become
        '''
      }
    }
  }
}
