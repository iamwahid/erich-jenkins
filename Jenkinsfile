def servers_list = ['103.157.96.146:22', '103.157.96.146:1111', '103.157.96.146:2222', '128.199.106.193:22']
def jenkins_credential = "server1_secret"
def jobs         = [:]

servers_list.each { sshServer ->
    jobs["jobs-${sshServer}"] = {
        node {
            stage("SSH to ${sshServer}") {
                script {
                    def serverHost = sshServer.split(":")[0]
                    def serverPort = sshServer.split(":")[1]
                    withCredentials([sshUserPrivateKey(credentialsId: "${jenkins_credential}", keyFileVariable: "keyfile", usernameVariable: "username")]) {
                
                    sh """
                        ssh -i '${keyfile}' -p $serverPort -o StrictHostKeyChecking=no -o CheckHostIP=no $username@$serverHost "ls /tmp"
                    """
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('SSH to servers') {
            steps {
                script {
                    parallel jobs
                }
            }
        }
    }
}