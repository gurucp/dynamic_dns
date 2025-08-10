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
        stage('Update Dynu DNS') {
            steps {
                // Use withCredentials to securely access the API key and expose it as an environment variable
                withCredentials([string(credentialsId: env.DYNU_CREDENTIALS_ID, variable: 'DYNU_API_KEY')]) {
                    script {
                        echo 'Running Ansible playbook to update Dynu DNS...'
                        // Run the Ansible playbook. The playbook will read the DYNU_API_KEY environment variable.
                        sh 'ansible-playbook -i inventory.ini update_dynu_dns.yml'
                    }
                }
            }
        }
    }
}
