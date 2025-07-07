pipeline {
    agent any
    triggers {
        pollSCM('H/2 * * * *')  // trigger for every 2 minutes (example)
    }
    environment {
        PORT = '9000'
        START_TIME = ''
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
                def commitEmails = ["rgundla@osidigital.com"]
                // Loop through changeSets and collect committer emails
                for (changeSet in currentBuild.changeSets) {
                    for (entry in changeSet.items) {
                        def email = entry.authorEmail
                        if (email && !commitEmails.contains(email)) {
                            commitEmails << email
                        }
                    }
                }

                def millis = currentBuild.duration
                def formatted = String.format('%02d:%02d:%02d',
                    (millis / (1000 * 60 * 60)) as int,
                    (millis / (1000 * 60) % 60) as int,
                    (millis / 1000 % 60) as int
                )

                emailext (
                    subject: "✅ SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>✅ Build succeeded for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                            <p><b>Duration:</b> ${formatted}</p>
                            <p>Check full log: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                    mimeType: 'text/html',
                    to: commitEmails.join(',')
                )
            }
        }

        failure {
            script {
                def commitEmails = ["rgundla@osidigital.com"]
                // Loop through changeSets and collect committer emails
                for (changeSet in currentBuild.changeSets) {
                    for (entry in changeSet.items) {
                        def email = entry.authorEmail
                        if (email && !commitEmails.contains(email)) {
                            commitEmails << email
                        }
                    }
                }
                def log = currentBuild.rawBuild.getLog(50).join('\n')
                def millis = currentBuild.duration
                def formatted = String.format('%02d:%02d:%02d',
                    (millis / (1000 * 60 * 60)) as int,
                    (millis / (1000 * 60) % 60) as int,
                    (millis / 1000 % 60) as int
                )

                emailext (
                    subject: "❌ FAILURE: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    body: """<p>❌ Build failed for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                            <p><b>Duration:</b> ${formatted}</p>
                            <p>Console Output (last 50 lines):</p>
                            <pre>${log}</pre>
                            <p>Check full log: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>""",
                    mimeType: 'text/html',
                    to: commitEmails.join(',')
                )
            }
        }

        always {
            echo "Pipeline completed."
        }

}
}