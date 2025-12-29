def branch = "main"
def remote = "origin"
def directory = "~/docker/wayshub-frontend"
def server = "fauzan@103.103.23.208"
def cred = "fauzanssh"

pipeline{
	agent any

	triggers {
		githubPush()
	}

	stages{
		stage('repo pull'){
		     steps{
			sshagent([cred]){
				sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
				cd ${directory}
				git pull ${remote} ${branch}
				exit
				EOF"""
				}
			}
		}

                stage('docker build'){
                     steps{
                        sshagent([cred]){
                                sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                                cd ${directory}
				docker build -t dumbflix-fe .
                                exit
                                EOF"""
                                }
                        }
                }

                stage('hapus container lama'){
                     steps{
                        sshagent([cred]){
                                sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
				docker rm -f frontend || true
                                exit
                                EOF"""
                                }
                        }
                }

                stage('docker run'){
                     steps{
                        sshagent([cred]){
                                sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
				docker run -d -p 3001:3000 --tty --name frontend dumbflix-fe
                                exit
                                EOF"""
                                }
                        }
                }
	}
}
