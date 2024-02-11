pipeline {
    agent any

    environment {
        // Define environment variables
        EC2_HOST = '52.66.243.132'
        EC2_USER = 'ubuntu'
        SSH_KEY_ID = '52.66.243.132'
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out from GitHub
                script {
                    git branch: 'main', url: 'https://github.com/farazghani12/Learn-Apache-Jenkins.git'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Use sshagent to handle credentials
                    sshagent(credentials: [SSH_KEY_ID]) {
                        // Copy files to the Apache directory on EC2
                        sh "scp -o StrictHostKeyChecking=no -r index.html ${EC2_USER}@${EC2_HOST}:/var/www/html/"
                        // Optional: Run any commands on EC2, like setting permissions or restarting Apache
                        sh "ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'sudo systemctl restart apache2'"
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
