3// pipeline {
//     agent any
//     stages {
//         stage('Example') {
//             steps {
//                 echo 'Hello World'
//             }
//         }
//     }
// }

// def remote = [:] remote.name = "node-1" remote.host = "185.50.203.10" remote.allowAnyHosts = true

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Build the up"
                sh 'mvn -e clean install'
                sh 'ls -l'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'tomcat-ssh', keyFileVariable: 'keyFile', passphraseVariable: 'passVar', usernameVariable: 'userVar')]) {
                        def remote = [
                            name: 'vm885e1c',
                            host: '185.50.203.10',
                            user: 'user1',
                            identityFile: keyFile,
                            allowAnyHosts: true
                        ]
                        sshCommand remote: remote, command: "ls -lrt"
                        sshCommand remote: remote, command: "for i in {1..5}; do echo -n \"Loop \$i \"; date; sleep 1; done"
                        writeFile file: 'abc.sh', text: '#!/bin/bash\nls -lrt'
                        sshScript remote: remote, script: 'abc.sh'
                        sshPut remote: remote, from: 'target/webApp1.war', into: '/opt/tomcat/webapps/'
                    }
                }
            }
        }
    }
}

