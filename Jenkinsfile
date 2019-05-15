
pipeline{
  agent any
    tools {
      maven 'Maven'
    }
   stages{
      stage('Prepare') {
            steps{
              script{
                checkout([$class: 'GitSCM', branches: [[name: '*/release/aws']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '6cea1f17-ffb7-4b36-8a41-155f94ce92bf', url: 'https://github.com/yellapantula/devops.git']]])
                }
            }
        }
       stage('Build') {
            steps{
                script{
                        sh"mvn --version"
                        if (isUnix()) {
                            sh " mvn clean deploy -Dmaven.test.failure.ignore"
                        } 
                    }
            }
       }
    }
        
 
}

      
