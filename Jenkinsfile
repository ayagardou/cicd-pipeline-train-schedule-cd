pipeline {
    agant any
    
    stage('Deploy to Staging') {
        when {
            branch 'master'
        }
        steps {
            withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                sshPublisher(
                    failOnError: true,
                    continueOnError: false,
                    publishers: [
                        configName: 'staging',
                        sshCredentials: [
                            username: "$USERNAME",
                            encryptedPassphrase: "$USERPASS"
                        ],
                        transfers: [
                            sshTransfer(
                                sourceFile: 'dist/trainSchedule.zip',
                                removePrefix: 'dist/',
                                remoteDirectory: '/tmp',
                                execCommand: 'sudo usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip - d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                            )
                        ]
                    ]
                )
            }
        }
    }
}
