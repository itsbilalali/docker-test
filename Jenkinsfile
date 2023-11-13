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
                    app = docker.build("nginx:latest")
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
                    docker tag nginx:latest 489994096722.dkr.ecr.us-west-1.amazonaws.com/nginx:11
                    docker.withRegistry('https://489994096722.dkr.ecr.us-west-1.amazonaws.com', 'ecr:us-west-1:aws-creds') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
