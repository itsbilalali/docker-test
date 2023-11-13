pipeline {
    agent none
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Check Branch and Build') {
            beforeAgent true
            steps {
                script {
                    def branchName = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    if (branchName == 'main') {
                        echo "Building on the main branch"
                        currentBuild.displayName = "Building on ${branchName}"
                        currentBuild.description = "Building on the main branch"
                        agent any
                    } else {
                        echo "Skipping build on branch: ${branchName}"
                        currentBuild.result = 'SUCCESS'
                        error("Not building on branch: ${branchName}")
                    }
                }
            }
            stages {
                stage('Clone repository') {
                    agent any
                    steps {
                        script {
                            checkout scm
                        }
                    }
                }

                stage('Build') {
                    agent any
                    steps {
                        script {
                            // Get the short Git commit hash (first 5 characters)
                            def commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                            app = docker.build("nginx:${commitHash}")
                        }
                    }
                }

                stage('Testing') {
                    agent any
                    steps {
                        echo 'Empty'
                    }
                }

                stage('Deploy') {
                    agent any
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
    }
}
