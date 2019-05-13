@Library('shared-library')_

pipeline{
  agent any

   stages{


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
   }
}

      
