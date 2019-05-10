@Library('shared-library')_

pipeline {
    agent any


   node {
      def mvnHome
      stage('Prepare') {
         git url: 'git@github.com:yellapantula/devops.git', branch: 'release/release_4'
         mvnHome = tool 'Maven'
      }
      stage('Build') {
         if (isUnix()) {
            sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
         } else {
            bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
      stage('Unit Test') {
         junit '**/target/surefire-reports/TEST-*.xml'
         archive 'target/*.jar'
      }
      stage('Integration Test') {
        if (isUnix()) {
           sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
        } else {
           bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
        }
      }
      stage('Sonar') {
         if (isUnix()) {
            sh "'${mvnHome}/bin/mvn' sonar:sonar"
         } else {
            bat(/"${mvnHome}\bin\mvn" sonar:sonar/)
         }
      }
      stage('Deploy') {
          sh 'curl -u jenkins:jenkins -T target/**.war "http://localhost:8080/manager/text/deploy?path=/devops&update=true"'
      }
      stage("Smoke Test"){
          sh "curl --retry-delay 10 --retry 5 http://localhost:8080/devops"
      }
   }
       post {
           always {
          /* Use slackNotifier.groovy from shared library and provide current build result as parameter */   
               slackNotifier(currentBuild.currentResult)
               cleanWs()
           }
       }

}
