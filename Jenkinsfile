pipeline {
    agent { label 'kthw-agent' }
    environment {
        JENKINS_NODE_COOKIE = 'do_not_kill'
        ANSIBLE_PRIVATE_KEY = credentials('admin-agent')
    }
    stages {
        stage('Run client_tools.yml') {
            steps {
                script {
                    // Run the first playbook
                    def result = sh(script: "ansible-playbook -i hosts.hosts --private-key=$ANSIBLE_PRIVATE_KEY client_tools.yml", returnStatus: true)

                    // Check if the first playbook ran successfully
                    if (result != 0) {
                        currentBuild.result = 'FAILURE'
                        error "client_tools.yml failed, skipping the next playbook."
                    }
                }
            }
        }
    }

    post {
        success {
            echo "All playbooks executed successfully!"
        }
        failure {
            echo "One or more playbooks failed."
        }
    }
}
