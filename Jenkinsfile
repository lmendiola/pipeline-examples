pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build demo-app'
        sh 'sh run_build_script.sh'
      }
    }

    stage('Linux test') {
      parallel {
        stage('Linux test') {
          steps {
            echo 'Run Linux tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('Windows tests') {
          steps {
            echo 'Run Windows tests'
          }
        }

      }
    }

    stage('Deploy staging') {
      steps {
        echo 'Deploy to staging environment in its cofiguration panel'
        input 'Ok to deploy to production?'
      }
    }

    stage('Deploy production') {
      steps {
        echo 'Deploy to PROD'
      }
    }

  }
  post {
    always {
      archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
    }

    failure {
      mail(to: 'laura.mendiolamartinez@fujitsu.com', subject: "Failed Pipeline ${currentBuild.fullDisplayName}", body: " For details about the failure, see ${env.BUILD_URL}")
    }

  }
}