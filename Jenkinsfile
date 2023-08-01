pipeline {
  agent {
    label 'riscv'
  }
  stages {

    stage('Installs JDK20') {
      tools {
        jdk "jdk19"
      }
      steps {
        withCredentials([string(credentialsId: 'GITHUB_CREDENTIALS_PSW', variable: 'GITHUB_CREDENTIALS_PSW')]) {
          sh '''echo $GITHUB_CREDENTIALS_PSW | gh auth login --with-token 
                git submodule init
                git submodule update
                ls -artl
                java --version
                cd temurin20-binaries
                echo "Removing previous binaries"
                rm -f "/home/jenkins/OpenJDK20U-jdk_riscv64_linux_hotspot_*.tar.gz"
                releaseName=$(gh release list | sed -n '1p' | sed 's/|/ /' | awk '{print $1}')
                echo "Downloading for $releaseName"
                gh release download $releaseName --pattern 'OpenJDK20U-jdk_riscv64_linux_hotspot_*.tar.gz' -D /home/jenkins
                cd /home/jenkins
                mainDirName=`tar -tzf OpenJDK20U-jdk_riscv64_linux_hotspot_*.tar.gz | head -1 | cut -f1 -d"/"`
                echo "We found $mainDirName as the main dir in the archive"
                rm -fr jdk-20* "$mainDirName" /home/jenkins/jdk20
                tar -xvzf /home/jenkins/OpenJDK20U-jdk_riscv64_linux_hotspot_*.tar.gz
                rm -f /home/jenkins/OpenJDK20U-jdk_riscv64_linux_hotspot_*.tar.gz
                ln -s "/home/jenkins/$mainDirName" /home/jenkins/jdk20
                ls -artl "/home/jenkins/$mainDirName"
'''
        }

      }
    }
  }
  options {
    timeout(time: 66, unit: 'MINUTES')
    // Add the cron syntax below to schedule the build every day at your desired time (in UTC)
    buildDiscarder(logRotator(numToKeepStr: '10'))
    // Schedule the build to run every day at 2:30 AM 
    cron('30 2 * * *')
  }
}
