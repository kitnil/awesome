pipeline {
    agent any
    triggers {
        cron("H 15 * * *")
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
