pipeline {
    agent any
    options {
        ansiColor('xterm')  // Enables colorized output
    }
    parameters {
        text(name: 'ADHOC_PLAYBOOK_CONTENT', defaultValue: '', description: 'Enter the adhoc playbook YAML content.')
    }

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg"
    }

    stages {
        stage('Run Adhoc Ansible Playbook') {
            steps {
                script {
                    def adhocPlaybookContent = params.ADHOC_PLAYBOOK_CONTENT.trim()

                    if (adhocPlaybookContent) {
                        def adhocPlaybookFile = "${WORKSPACE}/adhoc_playbook_temp.yml"

                        // Write the playbook content to a temporary file
                        writeFile file: adhocPlaybookFile, text: adhocPlaybookContent

                        sh """
                            ANSIBLE_CONFIG=${WORKSPACE}/ansible.cfg ansible-playbook -i inventory.yml ${adhocPlaybookFile}
                        """

                        // Optionally, delete the temporary file after execution
                        // sh "rm ${adhocPlaybookFile}"
                    } else {
                        echo "No adhoc playbook content provided."
                    }
                }
            }
        }
    }
}