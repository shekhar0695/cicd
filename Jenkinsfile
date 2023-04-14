pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/shekhar0695/cicd']])
                sh 'maven -Dmaven.test.failure.ignore=true clean package'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t shekharrr/maven .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub-pwd')]) {
                   sh 'docker login -u shekharrr -p ${dockerhub-pwd}'

        }
                   sh 'docker push shekharrr/maven'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubernetes')
                }
            }
        }
    }
}