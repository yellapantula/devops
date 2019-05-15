
pipeline{
  agent any
    tools {
      maven 'Maven'
    }
   stages{
      stage('Prepare') {
            steps{
            git url: 'git@github.com:yellapantula/devops.git', branch: 'release/aws'
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

      
