pipeline {
  agent any
  stages {
    stage('Init') {
      parallel {
        stage('Init') {
          steps {
            echo 'Hi, this is Anshul from LevelUp360'
            echo 'We are Starting the Testing'
          }
        }

        stage('unit test') {
          steps {
            sleep 10
          }
        }

        stage('code quality') {
          steps {
            mail(subject: 'completed', body: 'devenv', from: 'gc00479676@techmahindra.com', replyTo: 'gc00479676@TECHMAHINDRA.COM', to: 'gc00479676@techmahindra.com')
          }
        }

      }
    }

    stage('Build') {
      steps {
        echo 'Building Sample Maven Project'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying in Staging Area'
      }
    }

    stage('Deploy Production') {
      steps {
        echo 'Deploying in Production Area'
      }
    }

  }
}