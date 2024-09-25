# role-aap-object-credential
Ansible role to build AAP workflow objects

# Ref
https://console.redhat.com/ansible/automation-hub/repo/published/ansible/controller/content/module/schedule/

# How to use

Step 1: Install the role in your environment.
   - You could have roles/requirements.yml if running on AAP.
   - Or simple install on your environment.

Step 2: Define your variables in the structure below

  - build              : true/false # Bool value to switch role on off.
  - workflow_name      : Name of the workflow you want to build
  - inventory          : Name of the inventory to run against
  - organization       : Name of the organization
  - job_tmplts_list    : list of job templates to link into a worklow.
  - nodes_linking_list : lists of job_x <> job_y links to be linked ** See example**

Step 3: Call the role from your playbook.

# Example playbook

## varible definition in group_vars/*.yml

appc_config_backup_workflow: appc-network-config-backup-workflow

appc_config_backup_job_tmplts_list:
  -  "{{ fortios_config_backup_job_template_name }}"
  -  "{{ cisco_config_backup_job_template_name }}"
  -  "{{ junos_config_backup_job_template_name }}"
  -  "{{ f5_config_backup_job_template_name }}"

cisco_to_junos:
  - "{{ cisco_config_backup_job_template_name }}"
  - "{{ junos_config_backup_job_template_name }}"

junos_to_fortios:
  - "{{ junos_config_backup_job_template_name }}"
  -  "{{ fortios_config_backup_job_template_name }}"

fortios_to_f5:
  -  "{{ fortios_config_backup_job_template_name }}"
  -  "{{ f5_config_backup_job_template_name }}"
  
## playbook 

---
- name: Playbook to configure AAP
  hosts: localhost
  gather_facts: false
 
  pre_tasks:
    - name: Include specific project variables
      ansible.builtin.include_vars:
        dir: group_vars

  tasks:
    !
    !
    - name: Import role-aap-object-workflow-template
      ansible.builtin.include_role:
        name: role-aap-object-workflow-template
      vars:
        workflow: true
        workflow_name: "{{ appc_config_backup_workflow }}"
        inventory_name: "{{ inventory_name }}"
        organization_name: "{{ organization_name }}"
        job_tmplts_list: "{{ appc_config_backup_job_tmplts_list }}"
        nodes_linking_list: 
          - "{{ cisco_to_junos }}"
          - "{{ junos_to_fortios }}"
          - "{{ fortios_to_f5 }}"
    !
    !