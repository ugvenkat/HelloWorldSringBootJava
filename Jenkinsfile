

pipeline {
    tools {
        maven '3.6.3'
        jdk '17.0.7'
    }
    agent any
    stages {ster
        stage ('Java Check') {
            steps {
                sh 'java --version'
            }
        }
        stage ('Maven Check') {
            steps {
                sh 'mvn --version'
            }
        }

        stage ('Git Checkout') {
            steps {
                script{
                   //git branch: 'main', url:'https://github.com/ugvenkat/HelloWorldSringBootJava.git'
                   gitCheckout(
                            branch:"main"
                            url:"https://github.com/ugvenkat/HelloWorldSringBootJava.git"

                }
            }
        }

    } 
}
