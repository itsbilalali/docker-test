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
                    // Get the short Git commit hash (first 5 characters)
                    def commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    app = docker.build("nginx:${commitHash}")
                }
            }
        }

        stage('Testing') {
            steps {
                echo 'Empty'
            }
        }

        stage('Deploy') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }        
            steps {
                script {
                    docker.withRegistry('https://489994096722.dkr.ecr.us-west-1.amazonaws.com/nginx', 'ecr:us-west-1:aws-creds') {
                        // Push the Docker image with the commit hash as the tag
                        app.push("${commitHash}")
                    }
                }
            }
        }
    }
}
