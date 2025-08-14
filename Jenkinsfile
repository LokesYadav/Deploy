pipeline {
    agent any
    environment {
        PATH = "/Users/lokesh.kumar/Library/Python/3.9/bin/ansible-playbook"
        }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LokesYadav/Deploy.git'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Use Jenkins credentials for JFrog
                sh '''
                    ansible-playbook -i ansible/inventory.ini ansible/playbooks/deploy \
                    --extra-vars "jfrog_user=${JFROG_USER} jfrog_password=${JFROG_PASSWORD}"
                    '''

            }
        }
    }
}
