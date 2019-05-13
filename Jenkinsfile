@Library('shared-library')_

pipeline{
  agent any
    tools {
      maven 'Maven'
      }
   stages{
      stage('Prepare') {
          steps{
          git url: 'git@github.com:yellapantula/devops.git', branch: 'release/release_4'
          }
      }
        
   }
        post {
          always {
              script {
               cleanWs()
              SlackNotifier(currentBuild.currentResult)
              }
           }            
      }
}

      
