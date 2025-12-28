def branch = "master"
def remote = "origin"
def directory = "~/jenkins/dumbflix-frontend"
def server = "fauzan@103.103.23.208"
def cred = "fauzanssh"

pipeline{
	agent any
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
