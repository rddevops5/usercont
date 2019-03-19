#!/usr/bin/env groovy
DELIVER = (env.JOB_NAME as String).endsWith("-deliver")   

pipeline {
    agent {node'agent-01' } 
	
        stages {
		
		stage('Check syntax') {
		steps {
                withEnv(["ANSIBLE_HOST_KEY_CHECKING=False"]) {
                ansiblePlaybook(
                   playbook: 'ansible/site.yml',
                   inventory: 'ansible/inventory',
                   extras: '--syntax-check')
			   }
            }
       
		}

		stage('Ansible_Task') {
		when {
               
            expression { return DELIVER ==~ /(?i)(Y|YES|T|TRUE|ON|RUN)/ }   
              
           }
	 input {
                message "Should we continue?"
                ok "Yes, we should."
                //submitter "rauttdee"
                parameters {
                    choice(name: 'CHOICE', choices: ['Add_new_user', 'Changepassword', 'Generate_ssh_keys', 'Copy_pub_key', 'Grant_Sudo_to_group', 'Grant_sudo_Access', 'Revoke_sudo_Access', 'Add_new_group', 'Remove_Group', 'Remove_user'], description: 'Select Task from Dropdown')                }
            }
            steps {
                withEnv(["ANSIBLE_HOST_KEY_CHECKING=False"]) {
                ansiblePlaybook(
                    playbook: 'ansible/site.yml',
                    inventory: 'ansible/inventory',
                    credentialsId: 'temporary_root',
                    tags: "${CHOICE}",
					extras: '-v')
			   }
            }
        }
    }
}
