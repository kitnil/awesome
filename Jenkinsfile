def slackMessages = []

pipeline {
    agent { label "guixsd" }
    triggers {
        cron("H 15 * * *")
    }
    environment {
        GITHUB_TOKEN_STARRED = credentials('GITHUB_TOKEN_STARRED')
    }
    stages {
        stage("Update README.md") {
            steps {
                dir(library('jenkins-wi-shared-library').Constants.homeDir + "/src/awesome") {
                    script {
                        sh "starred --token=$GITHUB_TOKEN_STARRED --username wigust --sort > README.md"
                        catchError(buildResult: "SUCCESS", stageResult: "FAILURE") {
                            sh 'git commit -m "Update" README.md'
                        }
                        sh 'sshpass -Ppassphrase -p"$(pass show github/ssh/id_rsa_github)" git push github'
                        slackMessages += "wigust/awesome pushed to github.com"
                    }
                }
            }
        }
    }
    post {
        always {
            sendSlackNotifications (
                buildStatus: currentBuild.result,
                threadMessages: slackMessages
            )
        }
    }
}
