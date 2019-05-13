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
       stage('Build') {
        steps{
          script{
            if (isUnix()) {
                sh "-Dmaven.test.failure.ignore clean package"
            } 
          }

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

      
