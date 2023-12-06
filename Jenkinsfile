pipeline {
    agent any

    parameters {
        choice(name: 'BRANCH_TO_BUILD', choices: ['main', 'dev'], description: 'Select the branch to build')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: params.BRANCH_TO_BUILD]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/AhmedS0liman/jenkins_task2']]])
                }
            }
        }

        stage('Copy to Apache Server') {
            steps {
                script {
                    def remoteServer = '34.205.65.197' // Replace with your server's IP address
                    def remoteUser = 'ubuntu' // Replace with your SSH username
                    def remoteKeyPath = '/home/ubuntu/ansible.pem' // Replace with the path to your private SSH key file

                    // Define the local and remote paths
                    def localFilePath = 'index.html' // The file you want to copy
                    def remoteApachePath = '/var/www/html/index.html' // Replace with your Apache web server path

                    // Use SSH with private key to copy the file to the remote server
                    sshScript = """
                    ssh -o StrictHostKeyChecking=no -i ${remoteKeyPath} ${remoteUser}@${remoteServer} "sudo cp ${localFilePath} ${remoteApachePath}"
                    """
                    sh(script: sshScript, returnStatus: true)

                    // Restart Apache service on the remote server
                    sshScript = """
                    ssh -o StrictHostKeyChecking=no -i ${remoteKeyPath} ${remoteUser}@${remoteServer} "sudo systemctl restart apache2"
                    """
                    sh(script: sshScript, returnStatus: true)
                }
            }
        }
    }
}

