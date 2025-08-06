pipeline {
  agent any

  stages {
    stage('Lock One Build Per Branch') {
      steps {
        script {
          lock(resource: "lock-${env.JOB_BASE_NAME}", inversePrecedence: true) {
            echo "Running build for ${env.JOB_NAME} on branch ${env.BRANCH_NAME}"
          }
        }
      }
    }

    stage('Dummy Work') {
      steps {
        echo "Doing some work on ${env.BRANCH_NAME}..."
        sleep 300 // simulate work
      }
    }
    
  }

  post {
    always {
      echo "Build completed for ${env.JOB_NAME} on branch ${env.BRANCH_NAME}"
    }
  }
}
