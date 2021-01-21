#!groovy
string developmentJenkins = 'jenkins.com';
string stagingJenkins = 'sit';
string prodJenkins = '';

pipeline{
    agent any
    options {
        timeout(time: 30, unit: 'MINUTES')
        ansiColor('xterm')
    }
    environment {
        DEPLOY_CRED = credentials('AnsibleVault')
        NEXUS_URL = 'https://local.com'
    }
    parameters {
        choice( name: 'environment',
        description: 'Kindly choose Environment to deploy',
        choices: pipelineChoices()
            )
        string(
            name: 'package',
            description: 'Which package do you want to deploy?',
            defaultValue: 'vfde/omnius-ose-6.3.0-113.noarch.rpm'
        )
    }
    stages {
        stage('Performing Pre-Deployment Activity') {
            steps {
                script {
                    if (params.environment.contains("production")) {
                         echo params.environment+" Not supported at the movement" ;
                    } else {
                        sh """
                        cd ansible
                        export ANSIBLE_CONFIG=ansible.cfg
                        export ANSIBLE_FORCE_COLOR=true
                        echo \"${env.DEPLOY_CRED}\" > .vault_password
                        ansible-playbook -i inventories/"${params.environment}"/hosts playbooks/maintenance/pre_deployment.yml --vault-password-file .vault_password
                        sh """
                   }
                }
            }
        }
        stage('Performing Deployment Activity') {
            steps {
                script {
                    if (params.environment.contains("production")) {
                         echo params.environment+" Not supported at the movement" ;
                    } else {
                        env.extravars = "--extra-vars '{ \"ose\": { \"package\": \"${env.NEXUS_URL}/repository/rpm-packages/${params.package}\" } }'";
                        sh """
                        cd ansible
                        export ANSIBLE_CONFIG=ansible.cfg
                        export ANSIBLE_FORCE_COLOR=true
                        echo \"${env.DEPLOY_CRED}\" > .vault_password
                        ansible-playbook -i inventories/"${params.environment}"/hosts playbooks/maintenance/update_ose.yml ${env.extravars} --vault-password-file .vault_password
                        sh """
                    }
                }
            }
        }
