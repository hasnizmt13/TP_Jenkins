pipeline {
  agent any
  stages {

   stage ('Test') { // la phase build
steps {
sh './gradlew test'
 junit 'build/test-results/test/TEST-Matrix.xml'

   cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'My report',
                fileIncludePattern: 'target/report.json',

                trendsLimit: 10

}
}


        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              sh 'gradle sonar'
            }


          }
        }


         stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }


     stage('build') {


      steps {
        sh(script: 'gradle build', label: 'gradle build')
        sh 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)

      }
    }

     stage('Deploy') {
      steps {
        sh 'gradle publish'
      }
    }

     stage('Notification') {
      steps {

       notifyEvents message: 'New build is Created success', token: 'OlLQTQZ1_Wu3rvYmweXCHmbcu3DIJVYK'

      }
    }






}

  post {

        failure {

       notifyEvents message: 'New build faileddddd.', token: 'OlLQTQZ1_Wu3rvYmweXCHmbcu3DIJVYK'
        }


      }

 }
