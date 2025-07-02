pipeline {
    agent any
    // triggers {
    //     pollSCM('H/2 * * * *')  // trigger for every 2 minutes (example)
    // }
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
            }
        }
    }
}