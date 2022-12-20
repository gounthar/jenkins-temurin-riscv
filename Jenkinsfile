pipeline {
  agent any
  stages {

    stage('Shell script 0') {
      steps {
        withCredentials([string(credentialsId: 'GITHUB_CREDENTIALS_PSW', variable: 'GITHUB_CREDENTIALS_PSW')]) {
          sh '''echo $GITHUB_CREDENTIALS_PSW | gh auth login --with-token --scopes read:org
                git submodule init
                git submodule update
                ls -artl
                cd temurin20-binaries
                gh release list | sed -n '1p' | sed 's/|/ /' | awk '{print $1}'
'''
        }

      }
    }

  }
  options {
    timeout(time: 3, unit: 'MINUTES')
  }
}
