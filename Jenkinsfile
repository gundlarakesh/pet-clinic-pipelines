pipeline {
    agent any
    triggers {
        pollSCM('H/2 * * * *')  // trigger for every 2 minutes (example)
    }
    environment {
        PORT = '9000'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']], 
                    extensions: [], 
                    userRemoteConfigs: [[url: 'https://github.com/gundlarakesh/spring-petclinic.git']]
                )
            }
        }
        stage('Build') {
            steps {
                echo 'Building Sprint Petclinic..'
                bat 'mvn clean package -DskipTests'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing Sprint Petclinic..'
                bat 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying Sprint Petclinic..'
                // bat 'java -jar ./target/spring-petclinic-3.5.0-SNAPSHOT.jar --server.port=${PORT}'
            }
        }
    }
    post {
        success {
            script {
                def startTime = new Date(currentBuild.rawBuild.getStartTimeInMillis())
                def endTime = new Date()
                def durationMillis = endTime.time - startTime.time
                def durationFormatted = String.format('%02d:%02d:%02d',
                    (durationMillis / (1000 * 60 * 60)) as int,
                    (durationMillis / (1000 * 60) % 60) as int,
                    (durationMillis / 1000 % 60) as int
                )

                emailext (
                    subject: "✅ SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>✅ Build succeeded for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                            <p><b>Start Time:</b> ${startTime}</p>
                            <p><b>End Time:</b> ${endTime}</p>
                            <p><b>Total Duration:</b> ${durationFormatted}</p>
                            <p>Check full log: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                    mimeType: 'text/html',
                    to: 'rgundla@osidigital.com'
                )
            }
        }

        failure {
            script {
                def startTime = new Date(currentBuild.rawBuild.getStartTimeInMillis())
                def endTime = new Date()
                def durationMillis = endTime.time - startTime.time
                def durationFormatted = String.format('%02d:%02d:%02d',
                    (durationMillis / (1000 * 60 * 60)) as int,
                    (durationMillis / (1000 * 60) % 60) as int,
                    (durationMillis / 1000 % 60) as int
                )

                def log = currentBuild.rawBuild.getLog(50).join('\n')

                emailext (
                    subject: "❌ FAILURE: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    body: """<p>❌ Build failed for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                            <p><b>Start Time:</b> ${startTime}</p>
                            <p><b>End Time:</b> ${endTime}</p>
                            <p><b>Total Duration:</b> ${durationFormatted}</p>
                            <p>Console Output (last 50 lines):</p>
                            <pre>${log}</pre>
                            <p>Check full log: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                    mimeType: 'text/html',
                    to: 'rgundla@osidigital.com'
                )
            }
        }

        always {
            echo "Pipeline completed."
        }

}
}