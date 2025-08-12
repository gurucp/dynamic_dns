pipeline {
    agent any

    triggers {
        // Run every 5 minutes
        cron('H/5 * * * *')
    }

    environment {
        // Define the ID of the Jenkins credential for the Dynu API key
        DUCKDNS_TOKEN = 'DUCKDNS_TOKEN'
    }

    stages {
        stage('Checkout') {
            steps {
                // TODO: Replace with your actual repository URL
                git url: 'https://github.com/gurucp/dynamic_dns.git', branch: 'main'
            }
        }

        stage('Update Dynu DNS') {
            steps {
                withCredentials([string(credentialsId: env.DUCKDNS_TOKEN, variable: 'DUCKDNS_TOKEN')]) {
                    script {
                        echo 'Running Ansible playbook to update Dynu DNS...'
                        sh '''
                            ssh-keygen -f /var/lib/jenkins/.ssh/known_hosts -R trinetra.home.arpa || true
                            ssh-keyscan -H trinetra.home.arpa >> /var/lib/jenkins/.ssh/known_hosts
                            ssh-keygen -f /var/lib/jenkins/.ssh/known_hosts -R nataraja.home.arpa || true
                            ssh-keyscan -H nataraja.home.arpa >> /var/lib/jenkins/.ssh/known_hosts
                            ansible-playbook -i inventory.ini update_duckdns.yml
                        '''
                    }
                }
            }
        }
    }
}
