pipeline {
    agent any

    sshagent(['65.0.139.118']) {
    // some block
}
    stages {
        stage('Checkout') {
            steps {
                // Check out from GitHub
                git url: 'https://github.com/farazghani12/Learn-Apache-Jenkins.git', branch: 'main'
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Troubleshooting steps
                    echo '### Troubleshooting SSH and SCP ###'

                    // Step 1: Verify the content of the private key
                    withCredentials([sshUserPrivateKey(credentialsId: SSH_KEY_ID, keyFileVariable: 'PRIVATE_KEY')]) {
                        def privateKeyContent = env.PRIVATE_KEY
                        echo "Private Key Content: ${privateKeyContent}"

                        // Step 2: Print SSH agent environment variables
                        sshagent(credentials: [SSH_KEY_ID]) {
                            sh 'env | grep SSH'
                        }

                        // Step 3: Verify permissions on the target directory
                        sh "ssh -o StrictHostKeyChecking=no -i ${privateKeyContent} ${EC2_USER}@${EC2_HOST} 'ls -ld /var/www/html/'"

                        // Step 4: Verify permissions of the private key file
                        sh "ls -l ${privateKeyContent}"

                        // Step 5: Print the known_hosts file content
                        sh 'cat ~/.ssh/known_hosts'

                        // Deploy files to EC2
                        sshagent(credentials: [SSH_KEY_ID]) {
                            sh "scp -o StrictHostKeyChecking=no -r path/to/your/web/content ${EC2_USER}@${EC2_HOST}:/var/www/html/"
                            sh "ssh -o StrictHostKeyChecking=no -i ${privateKeyContent} ${EC2_USER}@${EC2_HOST} 'sudo systemctl restart apache2'"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
