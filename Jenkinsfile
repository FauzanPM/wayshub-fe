def branch = "main"
def remote = "origin"
def directory = "/home/fauzan/docker/wayshub-frontend"
def server = "fauzan@103.103.23.208"
def cred = "fauzanssh"

pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Repo Sync') {
            steps {
                sshagent([cred]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    git fetch ${remote}
                    git reset --hard ${remote}/${branch}
                    EOF
                    """
                }
            }
        }

        stage('Docker Build') {
            steps {
                sshagent([cred]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker build -t wayshub-fe:latest .
                    EOF
                    """
                }
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sshagent([cred]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${server} << EOF
                    docker stop wayshub-fe || true
                    docker rm wayshub-fe || true
                    EOF
                    """
                }
            }
        }

        stage('Run New Container') {
            steps {
                sshagent([cred]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${server} << EOF
                    docker run -d \\
                      --name wayshub-fe \\
                      -p 3001:3000 \\
                      wayshub-fe:latest
                    EOF
                    """
                }
            }
        }
    }
}

