pipeline {
  agent any

  options {
    // Avoid concurrent builds of the *same* branch
    disableConcurrentBuilds()
  }

  stages {
    stage('Ensure Only Latest Build Runs') {
      steps {
        script {
          def currentBuildNumber = currentBuild.number
          def jobName = env.JOB_NAME
          def job = Jenkins.instance.getItemByFullName(jobName)

          job.getBuilds().each { build ->
            // Skip current build
            if (build.number != currentBuildNumber && build.isBuilding()) {
              echo "Aborting older build #${build.number} of ${jobName}"
              build.doStop()
            }
          }
        }
      }
    }

    stage('Dummy Work') {
      steps {
        echo "Running on branch ${env.BRANCH_NAME}, build #${env.BUILD_NUMBER}"
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
