node {
    def slackResponse
    try {
        // Define your pipeline stages
        stage('Build') {
            slackResponse = slackSend(
                channel: "#andrewg-dev",
                tokenCredentialId: 'slack-andrewg-dev',
                message: "Build $JOB_NAME (#$BUILD_NUMBER) started.",
                color: "#0dcaf0"
            )

            // sleep(time: 5)
            sh 'docker --version'

            slackSend(
                channel: slackResponse.threadId,
                timestamp: slackResponse.ts,
                tokenCredentialId: 'slack-andrewg-dev',
                message: "Build $JOB_NAME (#$BUILD_NUMBER) completed successfully.",
                color: "good"
            )
        }
    } catch (Exception e) {
        slackSend(
            channel: slackResponse.threadId,
            timestamp: slackResponse.ts,
            tokenCredentialId: 'slack-andrewg-dev',
            message: "Build $JOB_NAME (#$BUILD_NUMBER) failed!",
            color: "danger"
        )
        slackSend(
            channel: slackResponse.threadId,
            tokenCredentialId: 'slack-andrewg-dev',
            message: e.message,
        )
    }
}
