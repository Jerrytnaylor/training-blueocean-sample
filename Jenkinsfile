pipeline {
  agent {
    docker {
      image 'bitwiseman/training-blueocean-sample'
      args '-u root -v $HOME/.m2:/root/.m2'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Test": {
            sh './jenkins/test-all.sh'
            
          },
          "Publish JUnit test result report": {
            sh '**/surefire-reports/**/*.xml'
            
          },
          "Publish JUnit test result report2": {
            sh '**/test-results/karma/*.xml'
            
          }
        )
      }
    }
  }
}