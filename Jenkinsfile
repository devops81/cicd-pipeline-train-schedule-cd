pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master'
                 }
            steps {
                withCredentialS([usernamePassword(credentialsId: 'webserver_loign',usernameVariable: 'USERNAME',passwordVariable:'USERPASS')]) {
                                                  sshPublisher(
                                                      failOnError: true,
                                                      continueOnError: false;
                                                      publishers: [
                                                          sshPublisherDesc(
                                                              configName: 'staging',
                                                              sshCredentials: [
                                                              useranme: "$username",
                                                              encryptedPassphrase: "$USERPASS"
                                                              ],
                                                              transfers: [
                                                                  sshTransfer(
                                                                      sourceFiles: 'dist/trainSchedule.zip',
                                                                      removePrefix: 'dist/',
                                                                      remoteDirectory: '/tmp',
                                                                      execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainschedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                                                      )
                                                                      ]
                                                                      ]
                                                                      ]
                                                                      }
                                                                      }
                                                                      }
                                                                      }
        
        
  
