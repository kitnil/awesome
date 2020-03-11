pipeline {
    agent any
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
    }
    post {
        always {
            sendNotifications currentBuild.result
        }
    }
}
