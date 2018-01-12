node {
  withCredentials([string(credentialsId: 'boundlessgeoadmin-token', variable: 'GITHUB_TOKEN'), string(credentialsId: 'sonar-jenkins-pipeline-token', variable: 'SONAR_TOKEN')]) {

    currentBuild.result = "SUCCESS"

    try {
      stage('Checkout'){
        checkout scm
          echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
      }

      stage('Package'){
        // make build
        sh """
          docker run -v \$(pwd -P):/web \
                     -w /web node:alpine sh \
                     -c 'apk add --no-cache build-base libcairo2-dev libjpeg8-dev libpango1.0-dev libgif-dev g++ bash \
                     && bash -c "npm prune"'
          """
      }

    }
    catch (err) {

      currentBuild.result = "FAILURE"
        throw err
    } finally {
      // Success or failure, always send notifications
      echo currentBuild.result
    }

  }
}
