pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/**/*.jar'
        archiveArtifacts 'build/docs/javadoc/**'
        junit 'build\\test-results\\test\\*.xml'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Mail Notification', body: 'La phase : Mail Notification .', cc: 'gm_begoug@esi.dz', to: 'ga_djamaa@esi.dz', from: 'abdelmalekdjamaa98@gmail.com')
      }
    }

    stage('Code Analysis') {
      steps {
        withSonarQubeEnv('sonar') {
          bat(script: 'gradle sonarqube', returnStatus: true)
          waitForQualityGate(webhookSecretId: 'ca116c31a71eb23bcf37cc02ff906a478d948d88') 
        }

      }
    }

    stage('Test reporting') {
      steps {
        jacoco(minimumBranchCoverage: '0.8')
      }
    }

    stage('Deployment') {
      steps {
        bat 'gradle publish'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(message: 'Deploiement est fini .')
      }
    }

  }
}
