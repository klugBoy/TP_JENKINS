pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar,build/docs/javadoc/**'
        junit 'build/test-results/test/*.xml'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Mail Notification', body: 'La phase : MAIL . ', cc: 'gm_begoug@esi.dz', to: 'ga_djamaa@esi.dz', from: 'abdelmalekdjamaa98@gmail.com')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonarqube'
            }

            waitForQualityGate true
          }
        }

        stage('Test reporting') {
          steps {
            jacoco(exclusionPattern: '**/test/*.class')
          }
        }

      }
    }

    stage('Deployment') {
      when {
        expression {
          env.CHANGE_ID == null
        }

      }
      steps {
        bat 'gradle publish'
      }
    }

    stage('Slack Notification') {
      when {
        expression {
          env.CHANGE_ID == null
        }

      }
      steps {
        slackSend(message: 'Deploiement est fini .')
      }
    }

  }
}