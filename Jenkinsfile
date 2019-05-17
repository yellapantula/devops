
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
                        if (isUnix()) {
                            sh " mvn clean deploy -Dmaven.test.failure.ignore"
                        } 
                  
                    }
            }
       }
        stage('Unit Test') {
            steps{
                junit '**/target/surefire-reports/TEST-*.xml'
                archive 'target/*.jar'
            }
        }
        stage('Integration Test') {
          steps{
              script{
                    if (isUnix()) {
                        sh "mvn clean verify -Dmaven.test.failure.ignore "
                    } 
                }
            }
        }
     
        stage('Sonar') {
          steps{
              script{
                    if (isUnix()) {
                        sh "mvn sonar:sonar  -Dsonar.projectKey=maven -Dsonar.host.url=http://ec2-3-89-20-110.compute-1.amazonaws.com:9000  -Dsonar.login=9b504928429270e986856bfff3b29325145f03bd"
                    }
                }
            }
        }
    }
        
 
}

      
