pipeline {
  agent any

  

  stages {
    stage('Abort Older Builds Across Branches') {
      steps {
        script {
          def currentJobName = env.JOB_NAME
          def currentBuildNumber = currentBuild.number
          def currentBranchName = env.BRANCH_NAME

          // Get parent folder (multibranch job name)
          def multibranchProjectName = currentJobName.split("/")[0]
          def multibranchProject = Jenkins.instance.getItem(multibranchProjectName)

          multibranchProject.getItems().each { branchJob ->
            branchJob.getBuilds().each { build ->
              def isCurrentBuild = (branchJob.fullName == currentJobName && build.number == currentBuildNumber)
              if (!isCurrentBuild && build.isBuilding()) {
                echo "Aborting build #${build.number} of branch ${branchJob.name}"
                build.doKill() // Force stop
              }
            }
          }
        }
      }
    }

    stage('Work') {
      steps {
        echo "Running branch ${env.BRANCH_NAME}, build #${env.BUILD_NUMBER}"
        sleep 300 // Simulated work
      }
    }
  }

  post {
    always {
      echo "Build complete for ${env.JOB_NAME} on branch ${env.BRANCH_NAME}"
    }
  }
}
