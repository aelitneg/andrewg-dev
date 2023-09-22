node {
  def slackResponse

  stage('Init') {
    try {
      slackResponse = slackSend(
        channel: "#andrewg-dev",
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Build $JOB_NAME <${env.BUILD_URL}|(#${env.$BUILD_NUMBER})> started",
        color: "#0dcaf0"
      )

      def checkoutObj = checkout([
        $class: 'GitSCM',
        branches: [[name: "main"]],
        userRemoteConfigs: [[
          credentialsId: 'andrewg_alitu',
          url: 'git@github.com:aelitneg/andrewg-dev.git'
        ]]
      ])

      def commits = sh(
        script: "git log --pretty=format:'%h - %an: %s' ${checkoutObj.GIT_PREVIOUS_SUCCESSFUL_COMMIT}...${checkoutObj.GIT_COMMIT}",
        returnStdout: true
      ).trim()

      slackSend(
        channel: slackResponse.threadId,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Changes:\n${commits}",
      )
    } catch(Exception e) {
      slackSend(
        channel: slackResponse.threadId,
        timestamp: slackResponse.ts,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Build $JOB_NAME <${env.BUILD_URL}|(#${env.$BUILD_NUMBER})> failed during Init stage!",
        color: "danger"
      )
      slackSend(
        channel: slackResponse.threadId,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Error: $e.message",
      )
    }
  }

  stage('Build') {
    try {
      slackSend(
        channel: slackResponse.threadId,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Starting Build stage",
      )

      sleep(5) // Do build things...
    } catch (Exception e) {
      slackSend(
        channel: slackResponse.threadId,
        timestamp: slackResponse.ts,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Build $JOB_NAME <${env.BUILD_URL}|(#${env.$BUILD_NUMBER})> failed during Build stage!",
        color: "danger"
      )
      slackSend(
        channel: slackResponse.threadId,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Error: $e.message",
      )
    }
  }

  stage('Deploy') {
    try {
      slackSend(
        channel: slackResponse.threadId,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Starting Deploy stage",
      )

      sleep(5) // Do deploy things...
      sh 'docker --version' // this will cause a failure

      slackSend(
        channel: slackResponse.threadId,
        timestamp: slackResponse.ts,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Build $JOB_NAME <${env.BUILD_URL}|(#${env.$BUILD_NUMBER})> completed successfully!",
        color: "good"
      )
    } catch (Exception e) {
      slackSend(
        channel: slackResponse.threadId,
        timestamp: slackResponse.ts,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Build $JOB_NAME <${env.BUILD_URL}|(#${env.$BUILD_NUMBER})> failed during Deploy stage!",
        color: "danger"
      )
      slackSend(
        channel: slackResponse.threadId,
        tokenCredentialId: 'slack-andrewg-dev',
        message: "Error: $e.message",
      )
    }
  }
}
