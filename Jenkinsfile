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
                releaseName=$(gh release list | sed -n '1p' | sed 's/|/ /' | awk '{print $1}')
                echo "Downloading for $releaseName"
                gh release download $releaseName --pattern 'OpenJDK20U-jdk_riscv64_linux_hotspot_*.tar.gz' --clobber -D /home/jenkins
'''
        }

      }
    }

  }
  options {
    timeout(time: 30, unit: 'MINUTES')
  }
}
