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
                dir("${Constants.homeDir}/src/awesome") {
                    sh "starred --token=$GITHUB_TOKEN_STARRED --username wigust --sort > README.md"
                    catchError(buildResult: "SUCCESS", stageResult: "FAILURE")    {
                        sh 'git commit -m "Update" README.md'
                    }
                }
            }
        }
        stage("Publish on the Internet") {
            when { branch "master" }
            steps {
                script {
                    comGithub.push (
                        group: Constants.githubOrganization,
                        name: "awesome"
                    )
                    slackMessages += "${GROUP_NAME}/${PROJECT_NAME} pushed to github.com"
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
