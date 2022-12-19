pipeline {
  agent any
  stages {
    stage('Checkout Scm') {
      steps {
        git 'https://github.com/adoptium/temurin20-binaries.git'
      }
    }

    stage('Shell script 0') {
      steps {
        withCredentials([string(credentialsId: 'GITHUB_CREDENTIALS_PSW', variable: 'GITHUB_CREDENTIALS_PSW')]) {
          sh '''echo $GITHUB_CREDENTIALS_PSW | gh auth login --with-token --scopes read:org
                gh release list'''
        }

      }
    }

  }
  options {
    timeout(time: 3, unit: 'MINUTES')
  }
}
