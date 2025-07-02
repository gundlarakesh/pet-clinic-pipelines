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
                bat 'java -jar ./target/spring-petclinic-3.5.0-SNAPSHOT.jar --server.port=$PORT'
            }
        }
    }
    post{ 
        always {
            echo 'Cleaning up..'
            bat 'mvn clean'
        }
    }

}