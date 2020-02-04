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
        
        failure { 
        mail(subject: 'Mail Notification', body: 'La phase : Mail Notification .', cc: 'gm_begoug@esi.dz', to: 'ga_djamaa@esi.dz', from: 'abdelmalekdjamaa98@gmail.com')
        }
        }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat(script: 'gradle sonarqube', returnStatus: true)
            }

            waitForQualityGate true
          }
        }
          

        stage('Test reporting') {
          steps {
            jacoco()
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
