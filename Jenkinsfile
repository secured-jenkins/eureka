pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages {
        stage('project build') {
            steps {
                script{
                    checkout scmGit(branches: [[name: '*/master']], browser: github('https://github.com/secured-jenkins/eureka.git'), extensions: [], userRemoteConfigs: [[url: 'https://github.com/secured-jenkins/eureka.git']])
                    def numOfRetry = 0;
                    retry(2) {
                        if(numOfRetry > 0){
                            sleep (20);
                        }
                        numOfRetry = numOfRetry + 1;
                        bat 'mvn clean install -P test'
                    }
                }
            }
        }
        stage('dockerize'){
            steps{
                bat 'docker build --tag hasanalrimawi/eureka-ci:%BUILD_NUMBER% .'
            }
        }
        stage('push image'){
            steps{
                bat 'docker push hasanalrimawi/eureka-ci:%BUILD_NUMBER%'
            }
        }
        stage('start Application'){
            steps{
                bat 'docker run -d --name eurekaServer --network com -p 8761:8761 hasanalrimawi/eureka-ci:%BUILD_NUMBER%'
            }
        }
    }
}