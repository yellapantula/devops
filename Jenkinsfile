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
                            sh " mvn clean package -Dmaven.test.failure.ignore"
                        } 
                    }
            }
       }
    }
        
 
}

      
