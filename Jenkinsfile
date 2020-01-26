pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/**/*.jar'
        archiveArtifacts 'build\\docs\\javadoc\\**'
        bat 'gradle check'
        junit 'build/reports/**/*.xml'
      }
    }

  }
}