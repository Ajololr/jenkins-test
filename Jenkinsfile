pipeline {
    agent { docker { image 'node:14-alpine' } }
    stages {
       stage('Deploy - Staging') {
            steps {
                sh './deploy staging'
                sh './run-smoke-tests'
            }
        }

        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }

        stage('Deploy - Production') {
            steps {
                sh './deploy production'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            mail to: 'ilya.androsau@itechart-group.com',
                subject: "Succeeded Pipeline: ${currentBuild.fullDisplayName}",
                body: "Everything is good with ${env.BUILD_URL}"
        }
        failure {
            mail to: 'ilya.androsau@itechart-group.com',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                body: "Something is wrong with ${env.BUILD_URL}"
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
