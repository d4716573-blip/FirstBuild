pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/d4716573-blip/FirstBuild.git'
            }
        }

        stage('Build Jar') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test Jar') {
            steps {
                sh 'java -jar target/simple-java-jenkins-1.0.0.jar'
            }
        }

        stage('Archive Jar') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
