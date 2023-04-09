@Library('my-shared-library') _

pipeline {
    agent any

    parameters{

        choice(name: 'action', choices:'create\ndelete', description:'choose create/Destory')
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
        
    }
}
