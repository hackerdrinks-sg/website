pipeline {

  agent any

  stages {
    stage('Build') {
      steps {
        checkout scm
        sh "git config --global user.name 'Jenkins CI'"
        sh "git config --global user.email 'jenkins-ci@example.com'"
        sh "ls"
        sh "git worktree add public master"
        sh "mkdir -p hackerdrinks/content"
        sh "cd hackerdrinks && hugo -F"
        sh "git status"
      }
    }
    stage('Build and Push Image') {
      environment{
          GIT_SSH_COMMAND="ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no"
        }
      when {
        branch 'source'
      }
      steps {
        // get the env
//        sh "ls"
//        sh "git worktree add public master"
//        sh "mkdir -p hackerdrinks/content"
//        sh "cd hackerdrinks && hugo"
//        sh "git status"
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

