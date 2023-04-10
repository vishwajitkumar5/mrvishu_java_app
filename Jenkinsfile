@Library('my-shared-library') _

pipeline {
    agent any

    parameters{

        choice(name: 'action', choices:'create\ndelete', description:'choose create/Destory')
        string(name: 'ImageName', description: "name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'vishuapp')
    }

    stages {
        
        
        stage('Checkout') {

            when{ expression{params.action == 'create'}}

            steps {
            gitCheckout(
                branch: "main",
                url: "https://github.com/vishwajitkumar5/mrvishu_java_app.git"
                )
               // checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/vishwajitkumar5/mrvishu_java_app.git']]])
            }
        }
        stage('Unit Test maven') {
            when{ expression{params.action == 'create'}}
            steps {
                script{
                    
                    mvnTest()
                }
            }
        }
        stage('Integration Test maven') {
            when{ expression{params.action == 'create'}}
            steps {
                script{
                    
                    mvnIntegrationTest()
                }
            }
        }
        stage('static code analysis: sonarqube') {
            when{ expression{params.action == 'create'}}
            steps {
                script{
                    
                    def SonarQubecredentialsId = 'sonarqube-api'
                    statiCodeAnalysis(SonarQubecredentialsId)
                }
            }
        }
        stage('quality gate status check: sonarqube') {
        when{ expression{params.action == 'create'}}
            steps {
                script{
                    
                    def SonarQubecredentialsId = 'sonarqube-api'
                    QualityGateStatus(SonarQubecredentialsId)
                }
            }
        }
        stage('Maven Build: maven') {
        when{ expression{params.action == 'create'}}
            steps {
                script{
                    
                    mvnBuild()
                }
            }
        }
        stage('Docker Image Build') {
        when{ expression{params.action == 'create'}}
            steps {
                script{
                    
                    dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                }
            }
        }
        
    }
}
