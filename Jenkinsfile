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

                        // YAML Validation
                        try {
                            sh "yamllint --strict ${adhocPlaybookFile}"
                            echo "YAML validation successful."
                        } catch (Exception e) {
                            def yamllintOutput = sh(script: "yamllint --strict ${adhocPlaybookFile}", returnStdout: true).trim()
                            error("YAML validation failed. Errors:\n${yamllintOutput}")
                        }
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