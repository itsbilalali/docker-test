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
                    commitId = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    app = docker.build("nginx:${commitId}")
                }
            }
        }

        stage('Testing') {
            steps {
                echo 'Empty'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Push the Docker image with the commit id as the tag
                    docker.withRegistry('https://489994096722.dkr.ecr.us-west-1.amazonaws.com/nginx', 'ecr:us-west-1:aws-creds') {
                        app.push("${commitId}")
                    }
                }
            }
        }
    }
}
