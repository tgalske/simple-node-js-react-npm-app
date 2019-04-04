pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }
  }

  environment {
    CI = 'true'

  }
  stages {

    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }

    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input message: 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }

    stage('Deploy') {
      steps {
        withAWS(region: 'us-east-1', credentials: 'tgalske-AWS-creds') {
          s3Upload(bucket: 'builds.thegc.cf', workingDir: '/var/jenkins_home/workspace/simple-react-app/build', includePathPattern:'**/*');
        }
      }
    }

  }
}
