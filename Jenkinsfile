pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg"
    }

    stages {
        stage('Run Ansible Playbook') {
            steps {
                script {
                    def inventoryFile = "${WORKSPACE}/inventory.yml"
                    def playbookFile = "${WORKSPACE}/adhoc/playbook.yml" // Replace with your playbook name.

                    sh """
                        ansible-playbook -i ${inventoryFile} ${playbookFile}
                    """
                }
            }
        }
    }
}