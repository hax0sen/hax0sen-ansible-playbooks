pipeline {
    agent any
    options {
        ansiColor('xterm') // Enables colorized output
    }
    parameters {
        text(name: 'ADHOC_PLAYBOOK_CONTENT', defaultValue: '', description: 'Enter the adhoc playbook YAML content.')
    }

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg"
    }

    stages {
        stage('Validate YAML') {
            steps {
                script {
                    def adhocPlaybookContent = params.ADHOC_PLAYBOOK_CONTENT.trim()

                    if (adhocPlaybookContent) {
                        def adhocPlaybookFile = "${WORKSPACE}/adhoc_playbook_temp.yml"
                        writeFile file: adhocPlaybookFile, text: adhocPlaybookContent

                        // // YAML Validation using returnStatus to handle errors
                        // def lintStatus = sh(script: "yamllint ${adhocPlaybookFile}", returnStatus: true)
                        // if (lintStatus != 0) {
                        //     error("YAML validation failed. Please check your playbook syntax.")
                        // } else {
                        //     echo "YAML validation successful."
                        // }
                    } else {
                        echo "No adhoc playbook content provided."
                    }
                }
            }
        }
        stage('Run Adhoc Ansible Playbook') {
            steps {
                script {
                    def adhocPlaybookFile = "${WORKSPACE}/adhoc_playbook_temp.yml"
                    def adhocPlaybookContent = params.ADHOC_PLAYBOOK_CONTENT.trim()

                    if (adhocPlaybookContent) {
                        sh """
                            ANSIBLE_CONFIG=${WORKSPACE}/ansible.cfg ansible-playbook --syntax-check -i inventory.yml ${adhocPlaybookFile}
                            ANSIBLE_CONFIG=${WORKSPACE}/ansible.cfg ansible-playbook -i inventory.yml ${adhocPlaybookFile}
                        """

                        // Optionally, delete the temporary file after execution
                        // sh "rm ${adhocPlaybookFile}"
                    }
                }
            }
        }
    }
}
