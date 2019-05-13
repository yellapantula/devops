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
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

        stage('Deploy') {
            steps{
                sh 'curl -u jenkins:jenkins -T target/**.war "http://localhost:8080/manager/text/deploy?path=/devops&update=true"'
            }
        }
        stage("Smoke Test"){
            steps{
                sh "curl --retry-delay 10 --retry 5 http://localhost:8080/devops"
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

      
