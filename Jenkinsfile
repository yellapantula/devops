@Library('shared-library')_
def mvnHome= tool 'Maven'

pipeline{
  agent any
   stages{
      stage('Prepare') {
          steps{
           node ('master'){
           git url: 'git@github.com:yellapantula/devops.git', branch: 'release/release_4'
           }
          }
      }
   }
}

      
