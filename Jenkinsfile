pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'phase build'
        sh './gradle javadoc'
        archiveArtifacts 'Documentation'
      }
    }

  }
}