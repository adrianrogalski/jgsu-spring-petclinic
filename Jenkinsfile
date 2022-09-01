pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }

        }
    }
    post {
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
        }
        changed {
            emailext attachLog: true,
            body: 'Please go to ${BUILD_URL}',
            compressLog: true,
            recipientProviders: [upstreamDevelopers(),
            requestor()],
            subject: "Job \'${JOB_NAME}\' status: ${currentBuild.result}",
            to: 'adrianrogalski07@gmail.com'
        }
    }
}
