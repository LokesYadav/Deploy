pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LokesYadav/Deploy.git'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Use Jenkins credentials for JFrog
                withCredentials([usernamePassword(credentialsId: 'JFROG_CRED', usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_PASSWORD')]) {
                    sh '''
                    ansible-playbook -i ansible/inventory.ini ansible/playbooks/deploy \
                    --extra-vars "jfrog_user=${JFROG_USER} jfrog_password=${JFROG_PASSWORD}"
                    '''
                }
            }
        }
    }
}
