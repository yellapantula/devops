@Library('shared-library')_
def mvnHome=tool 'Maven'

pipeline {
    agent any
    node{
        stages {          
         stage('Prepare') {
            steps{
            node ('master')
            git url:'git@github.com:yellapantula/devops.git', branch: 'release/release_4'
            }
         }
         stage('Build') {
            steps{
            node ('master')
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
            node ('master')
               junit '**/target/surefire-reports/TEST-*.xml'
               archive 'target/*.jar'
            }
         }
         stage('Integration Test') {
            steps{
            node ('master')
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
            node ('master')
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
            node ('master')
               script{
                sh 'curl -u jenkins:jenkins -T target/**.war "http://localhost:8080/manager/text/deploy?path=/devops&update=true"'
               }
            }
         }
         stage("Smoke Test"){
            steps{
            node ('master')
               script{
                    sh "curl --retry-delay 10 --retry 5 http://localhost:8080/devops"
               }
            }
          }
        }
    }
    post {
        always {
            cleanWs()
            SlackNotifier(currentBuild.currentResult)
            
        }            
    }
}
