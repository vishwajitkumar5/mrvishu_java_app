@Library('my-shared-library') _

pipeline {
    agent any

    parameters{

        choice(name: 'action', choices:'create\ndelete', descripation:'choose create/Destory')
    }

    stages {
        
        when{ expression{param.action == 'create'}}
        stage('Checkout') {
            steps {
            gitCheckout(
                branch: "main",
                url: "https://github.com/vishwajitkumar5/mrvishu_java_app.git"
                )
               // checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/vishwajitkumar5/mrvishu_java_app.git']]])
            }
        }
        stage('Unit Test maven') {
            when{ expression{param.action == 'create'}}
            steps {
                script{
                    
                    mvnTest()
                }
            }
        }
        stage('Integration Test maven') {
            when{ expression{param.action == 'create'}}
            steps {
                script{
                    
                    mvnIntegrationTest()
                }
            }
        }
        stage('static code analysis: sonarqube') {
            when{ expression{param.action == 'create'}}
            steps {
                script{
                    
                    statiCodeAnalysis()
                }
            }
        }
        
    }
}
