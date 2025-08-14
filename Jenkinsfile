// pipeline {
//     agent any
//     environment {
//         PATH = "/Users/lokesh.kumar/Library/Python/3.9/bin:${PATH}"
//         }
//     stages {
//         stage('Checkout') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/LokesYadav/Deploy.git'
//             }
//         }
    
//         stage('Run Ansible Playbook') {
//             steps {
//                 // Use Jenkins credentials for JFrog
//                 sh '''
//                     ansible-playbook -i ansible/inventory.ini ansible/playbooks/deploy \
//                     --extra-vars "jfrog_user=${JFROG_USER} jfrog_password=${JFROG_PASSWORD}"
//                     '''

//             }
//         }
//     }
// }


pipeline {
    agent any
    environment {
        PATH = "/Users/lokesh.kumar/Library/Python/3.9/bin:${PATH}"
    }
    
    stages {

stage('Install sshpass if missing') {
    steps {
        sh '''
        if ! command -v sshpass >/dev/null 2>&1; then
            echo "🔍 sshpass not found — installing..."
            if [[ "$OSTYPE" == "darwin"* ]]; then
                /usr/local/bin/brew install hudochenkov/sshpass/sshpass
            elif [ -f /etc/debian_version ]; then
                sudo apt update && sudo apt install -y sshpass
            elif [ -f /etc/redhat-release ]; then
                sudo yum install -y sshpass
            else
                echo "❌ Unsupported OS — please install sshpass manually."
                exit 1
            fi
        else
            echo "✅ sshpass already installed."
        fi
        '''
    }
}


        
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/LokesYadav/Deploy.git'
            }
        }
    
        stage('Run Ansible Playbook') {

steps {
                withCredentials([
                    usernamePassword(credentialsId: 'ansible-ssh-creds', usernameVariable: 'SSH_USER', passwordVariable: 'SSH_PASS'),
                    usernamePassword(credentialsId: 'JFROG_CRED', usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_PASSWORD')
                ]) {
                    sh """
                        ansible-playbook -i ansible/inventory.ini ansible/deploy.yml \
                        -u $SSH_USER \
                        --extra-vars "ansible_password=$SSH_PASS ansible_become_password=$SSH_PASS" \
                        --extra-vars "jfrog_user=$JFROG_USER jfrog_password=$JFROG_PASSWORD"
                    """
                }
            }
        }
    }
}
