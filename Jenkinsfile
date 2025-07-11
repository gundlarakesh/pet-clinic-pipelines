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
        // stage('Test') {
        //     steps {
        //         echo 'Testing Sprint Petclinic..'
        //         bat 'mvn test'
        //     }
        // }
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
                emailext (
                    subject: "✅ SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """<p>✅ Build succeeded for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                             <p><a href="${env.BUILD_URL}console">Click here</a> to check full log</p>""",
                    mimeType: 'text/html',
                    to: "rgundla@osidigital.com"
                )
            }
        }

        failure {
            script {
                def log = currentBuild.rawBuild.getLog(50).join('\n')

                emailext (
                    subject: "❌ FAILURE: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    body: """<p>❌ Build failed for <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b>.</p>
                            <p>Console Output (last 50 lines):</p>
                            <pre>${log}</pre>
                            <p><a href="${env.BUILD_URL}console">Click here</a> to check full log</p>
                            """,
                    mimeType: 'text/html',
                    to: "rgundla@osidigital.com"
                )
            }
        }

        always {
            echo "Pipeline completed."
        }
}
}