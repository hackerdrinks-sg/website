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
  containers:
  - name: dind
    image: jojomi/hugo:latest
#    image: docker:18.05-dind
    command:
    - cat
    tty: true
    env:
    - name: DOCKER_HOST
      value: "tcp://192.168.1.34:2375"
"""

    }
  }

  stages {
    stage('Build and Push Image') {
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
          sh "cd public && ls && git add ."
          sh "cd public && git commit -m 'test me'"
          sh "git log master --graph --decorate --pretty=oneline --abbrev-commit"
          sh "cd public && git push origin master"
					sh "echo Workspace dir is ${pwd()}"
      }
    }
  }
}
