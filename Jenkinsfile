@Library('shared-library')_

pipeline{
  agent any

   stages{
      stage('Prepare') {
          steps{
          git url: 'git@github.com:yellapantula/devops.git', branch: 'release/release_4'
          }
      }
      stage('Build') {
        def mvnHome= tool 'Maven'
        steps{
            script{
              if (isUnix()) {
                  sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
              } else {
                  bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
              }
            }
             
          }
       }
        
   }
}

      
