@Library('my-shared-library') _

pipeline{

    agent any

    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'AppName', description: "AppName/ImageName", defaultValue: 'springbootApp')
        string(name: 'ImageTag', description: "Docker Tag Image Name", defaultValue: 'latest')
        string(name: 'DockerHubUser', description: "docker userid", defaultValue: 'ugvenkat')
    }

    stages{

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
         
        stage('Git Checkout'){
                    when { expression {  params.action == 'create' } }
            steps{

            	//git branch: 'main', url:'https://github.com/ugvenkat/HelloWorldSringBootJava.git'
            gitCheckout(
                branch: "main",
                url: "https://github.com/ugvenkat/HelloWorldSringBootJava.git"
            )
            }
        }
         stage('Unit Test maven'){
         
         when { expression {  params.action == 'create' } }

            steps{
               script{
                   
                   mvnTest()
               }
            }
        }
         stage('Integration Test maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnIntegrationTest()
               }
            }
        }
        stage('Static code analysis: Sonarqube'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   def SonarQubecredentialsId = 'sonarqube-api'
                   statiCodeAnalysis(SonarQubecredentialsId)
               }
            }
        }
        stage('Quality Gate Status Check : Sonarqube'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   def SonarQubecredentialsId = 'sonarqube-api'
                   QualityGateStatus(SonarQubecredentialsId)
               }
            }
        }
        stage('Maven Build : maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnBuild()
               }
            }
        }
        stage('Docker Image Build'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerBuild("${params.DockerHubUser}","${params.AppName}","${params.ImageTag}")
               }
            }
        }


         stage('Docker Image Scan: trivy '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImageScan("${params.DockerHubUser}","${params.AppName}","${params.ImageTag}")
               }
            }
        }


        stage('Push DockerHub') {
            environment {
              DOCKERHUB_CREDENTIALS = credentials('dockerhub_id')
            }
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh "docker image push ${params.DockerHubUser}/${params.AppName}:${params.ImageTag}"
            }
        }
    }  // Stages

    post {
        always {
           sh 'docker logout'
        }
    }

/*

        stage('Docker Image Push : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }   
        stage('Docker Image Cleanup : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImageCleanup("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }   
*/   


    
} //Pipeline
