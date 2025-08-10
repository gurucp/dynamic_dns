pipeline {
    agent any

    triggers {
        // Run every 5 minutes
        cron('H/5 * * * *')
    }

    environment {
        // Define the ID of the Jenkins credential for the Dynu API key
        DYNU_CREDENTIALS_ID = 'dynu_api'
    }

    stages {
        stage('Checkout') {
            steps {
                // TODO: Replace with your actual repository URL
                git url: 'https://github.com/your-username/your-repository.git', branch: 'main'
            }
        }

        stage('Update Dynu DNS') {
            steps {
                withCredentials([string(credentialsId: env.DYNU_CREDENTIALS_ID, variable: 'DYNU_API_KEY')]) {
                    script {
                        echo 'Running Ansible playbook to update Dynu DNS...'
                        sh 'ansible-playbook -i inventory.ini update_dynu_dns.yml'
                    }
                }
            }
        }
    }
}