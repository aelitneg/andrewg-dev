node {
    def slackResponse
    try {
        stage('Build') {
            slackResponse = slackSend(
                channel: "#andrewg-dev",
                tokenCredentialId: 'slack-andrewg-dev',
                message: "Build $JOB_NAME (#$BUILD_NUMBER) started.",
                color: "#0dcaf0"
            )

            def commitMessages = []

            def commits = checkout([
              $class: 'GitSCM',
              branches: [[name: "main"]],
              userRemoteConfigs: [[
                credentialsId: 'andrewg_alitu',
                url: 'git@github.com:aelitneg/andrewg-dev.git'
              ]]
            ])

            sh "git log --pretty=format:'%s' ${commits.GIT_PREVIOUS_SUCCESSFUL_COMMIT}...${commits.GIT_COMMIT}"

            slackSend(
                channel: slackResponse.threadId,
                timestamp: slackResponse.ts,
                tokenCredentialId: 'slack-andrewg-dev',
                message: "Build $JOB_NAME (#$BUILD_NUMBER) completed successfully.",
                color: "good"
            )
        }
    } catch (Exception e) {
        echo "catching exception!"
        // echo "$e"

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
