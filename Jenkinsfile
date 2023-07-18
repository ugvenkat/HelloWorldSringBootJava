pipeline {
    tools {
        maven '3.6.3'
        jdk '17.0.7'
    }
    agent any
    stages {
        stage ('Java Version') {
            steps {
                sh 'java --version'
            }
        }
        stage ('Maven Version') {
            steps {
                sh 'mvn --version'
            }
        }
        
    }
}
