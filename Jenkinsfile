@Library('shared-library')_
def mvnHome= tool 'Maven'

pipeline{
  agent any
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
                    sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
                } else {
                    bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
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
                sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
                } else {
                bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
                }
              }
          }
      }
      stage('Sonar') {
          steps{
              script{
                if (isUnix()) {
                    sh "'${mvnHome}/bin/mvn' sonar:sonar"
                } else {
                    bat(/"${mvnHome}\bin\mvn" sonar:sonar/)
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
            cleanWs()
            slackNotifier(currentBuild)
        }
      }
}



