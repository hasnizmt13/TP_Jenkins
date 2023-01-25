  pipeline {
    agent any
    stages {
        stage ('test') { // la phase build is
            steps {
                bat 'gradlew test'
                archiveArtifacts 'build/test-results/'
                cucumber reportTitle: 'Cucumber report',
                fileIncludePattern: 'target/report.json',
                trendsLimit: 10,
                classifications: [
                    [
                       'key': 'Browser',
                        'value': 'Firefox'
                    ]
                ]
                junit 'build/test-results/test/TEST-Matrix.xml'
            }

         }
        
          stage ('Code Analysis') { // la phase build
            steps {
                                withSonarQubeEnv('sonar'){
                bat 'gradle sonarqube'
                                }
            }
         }
          stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
                  stage("Build") {
            steps {
                bat 'gradle build'
                bat 'gradle javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }
             stage("deploy") {
            steps {
                bat 'gradle publish'

            }
        }
                         stage("notification") {
            steps {
                 notifyEvents message: 'Pipeline <b> is sucessufuly termined</b>', token: '_HDOPWM66V8CyRupbxsIWff0BGHh06p7'

            }
        }

    }
//             post {

//         failure {
//             mail bcc: '', body: '''process Failed!!!!
// Soory chamsou''', cc: '', from: '', replyTo: '', subject: 'process Faild', to: 'jh_zoumata@esi.dz'
//         }

// }


    }

