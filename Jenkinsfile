pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'phase build'
        bat 'gradle javadoc'
      }
    }

  }
}