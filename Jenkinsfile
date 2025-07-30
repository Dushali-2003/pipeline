pipeline {
    agent any

    stages {
        stage('Delete the workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Installing ansible') {
            steps {
                script {
                    def ansible_exists = fileExists '/usr/bin/ansible'
                    if (ansible_exists == true) {
                        echo "Skipping Ansible install - already exists"
                    } else {
                        sh 'sudo apt-get update -y && sudo apt-get upgrade -y'
                        sh 'sudo apt install -y wget tree unzip ansible python3-pip python3-apt'
                    }
                }
            }
        }
        stage('Download ansible code') {
            steps {
                git credentialsId: 'c2a4b92c-4416-4678-a110-60af51634bed', url: 'git@github.com:Dushali-2003/pipeline.git'
            }
        }
        stage('Run ansible-lint against playbook') {
            steps {
                sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 apache-install.yml'
                sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 website-update.yml'
                sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 website-test.yml'
    }
}
