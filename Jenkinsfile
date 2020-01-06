pipeline {
    agent any
    triggers {
        cron("0 21 * * *")
    }
    stages {
        stage("Update README.md") {
            steps {
                dir("${Constants.homeDir}/src/awesome") {
                    sh "make"
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
