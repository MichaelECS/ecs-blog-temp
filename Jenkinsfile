@Library('jenkins_shared') _

parameters {
  string(name: 'fieldName', defaultValue: "index.html", description: "Our file")
}

pipeline {
    agent any

  options {
    timestamps()
  }

    stages {
        stage ("Build") {
          steps {
            echo "Building..."
            replaceString("index.html", "${BUILD_NUMBER}")
            helloWorld("Michael", "Test")
          }
        }
        stage ("Test") {
          parallel {
            stage ("Test Windows") {
              steps {
                echo "Testing Windows..."
                sh "cat index.html"
                sh 'grep -c "<p><small>Deployed by Jenkins job: [0-9+]" index.html'
              }
            }
            stage ("Test Linux") {
              steps {
                echo "Testing Linux..."
              }
            }
          }
        }
        stage ("Deploy") {
          steps {
            echo "Deploying..."
            sshPublisher(publishers: [sshPublisherDesc(configName: 'web', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'mv index.html /var/www/html', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'index.html')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
          }
        }
    }
    post {
        always {
            echo 'One way or another, I have finished'
            //archiveArtifacts artifacts: 'index.html', followSymlinks: false
        }
    }
}
