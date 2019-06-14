pipeline {
  agent {
    kubernetes {
      label 'dind'
      defaultContainer 'dind'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  imagePullSecrets:
  - name: gitlab-homelab
  containers:
  - name: dind
    image: registry.gitlab.com/davidchua/homelab/hugo-builder:latest
#    image: docker:18.05-dind
    command:
    - cat
    tty: true
    env:
    - name: DOCKER_HOST
      value: "tcp://[2401:7400:8000:0:3:0:c0a8:0122]:2375"
"""

    }
  }

  stages {
    stage('Build and Push Image') {
      environment{
			    GIT_SSH_COMMAND="ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no"
        }
			steps {
        checkout scm
					// get the env
					sh "git config --global user.name 'Jenkins CI'"
					sh "git config --global user.email 'jenkins-ci@example.com'"
					sh "ls"
					sh "git worktree add public master"
					sh "mkdir -p hackerdrinks/content"
          sh "cd hackerdrinks && hugo"
          sh "git status"
          dir("public"){
          	sshagent(['githubssh']) {
							sh "git add ."
							sh "git commit --allow-empty -m 'built from: ${env.GIT_COMMIT}'"
							sh "git remote rm origin"
							sh "git remote add origin git@github.com:hackerdrinks-sg/website.git > /dev/null 2>&1"   
							sh "git push origin master"
							sh "git log master --graph --decorate --pretty=oneline --abbrev-commit"
  					 }
          }
					sh "echo Workspace dir is ${pwd()}"
      }
    }
  }
}

