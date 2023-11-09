pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script {
                    checkout scm
                }
            }
        }

        stage('Build') { 
            steps { 
                script { 
                    app = docker.build("${DOCKER_IMAGE}:${TAG}")
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Empty'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('489994096722.dkr.ecr.us-west-1.amazonaws.com/bilal-test', 'ecr:us-west-1:aws-credentials') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
