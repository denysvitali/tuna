pipeline {
  agent any
  environment {
    CRED = credentials('supsi-scm-denvit')
  }
  stages {
    stage("Slack Notification"){
      steps {
        slackSend color: '#2c3e50', message: "*${env.JOB_NAME}*\nStarted build <${BUILD_URL}|#${BUILD_NUMBER}> for ${JOB_NAME} (<https://git.ded1.denv.it/shrug/tuna/commit/${GIT_COMMIT}|${GIT_COMMIT}>) on branch $GIT_BRANCH."
      }
    }

    stage("Cleanup"){
      steps {
        sh "git clean -fdx"
      }
    }

    stage("Submodules Checkout"){
      steps {
        sh "git submodule init"
        sh "git submodule sync"
        sh "git submodule update"
      }
    }

    stage("Build Lib"){
      steps {
        script {
          docker.image('dvitali/tuna-builder:latest').inside() {
            sh "mkdir cmake-build-debug/"
            sh "cd cmake-build-debug && cmake ../"
            sh "cd cmake-build-debug && make"
          }
        }
      }
    }
  }
  post {
    failure {
        slackSend color: 'danger', message: "*${env.JOB_NAME}*\nBuild <${BUILD_URL}|#${BUILD_NUMBER}> *failed*! Branch $GIT_BRANCH, commit: (<https://git.ded1.denv.it/shrug/tuna/commit/${GIT_COMMIT}|${GIT_COMMIT}>). :respects:\n\n*Commit Log*:\n${COMMIT_LOG}"
    }
    success {
        slackSend color: 'good', message: "*${env.JOB_NAME}*\nBuild <${BUILD_URL}|#${BUILD_NUMBER}> *successful*! Branch $GIT_BRANCH, commit: (<https://git.ded1.denv.it/shrug/tuna/commit/${GIT_COMMIT}|${GIT_COMMIT}>). :tada: :blobdance: :clappa:\n\n*Commit Log*:\n${COMMIT_LOG}"
    }
    always {
        script {
          try {
            COMMIT_LOG = sh(script:"git log --oneline --pretty=format:'%h - %s (%an)' ${GIT_PREVIOUS_COMMIT}..HEAD", returnStdout: true)
          } catch(e){
            COMMIT_LOG = ""
          }
        }
        sh "git remote remove supsi | true"
        sh "git remote add supsi -f https://$CRED@scm.ti-edu.ch/repogit/labingsw022018201907supsige.git | true"
        sh "git push -u supsi origin/$GIT_BRANCH"
        sh "git push --tags supsi origin/$GIT_BRANCH"
        archiveArtifacts artifacts: 'cmake-build-debug/tuna-gui/tuna_gui'
        archiveArtifacts artifacts: 'cmake-build-debug/tuna-ge/libtuna_ge.a'
        cleanWs()
    }
  }
}
