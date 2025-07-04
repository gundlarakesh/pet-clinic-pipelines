pipeline {
    agent any
    // triggers {
    //     pollSCM('H/2 * * * *')  // trigger for every 2 minutes (example)
    // }
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
            emailext (
                subject: "✅ SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Good news! Build succeeded.\n\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}",
                to: 'rgundla@osidigital.com'
            )
        }

        failure {
            emailext (
                subject: "❌ FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Build failed.\n\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}",
                to: 'rgundla@osidigital.com'
            )
        }

        always {
            echo "Pipeline completed."
        }

}