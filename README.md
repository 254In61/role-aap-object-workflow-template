# role-aap-object-credential
Ansible role to build AAP workflow objects

# Ref
https://console.redhat.com/ansible/automation-hub/repo/published/ansible/controller/content/module/schedule/


# design
- Role takes in a list and runs with it.

# How to use

Step 1: Install the role in your environment.
   - You could have roles/requirements.yml if running on AAP.
   - Or simple install on your environment.

Step 2: Define your variables in the structure below

workflow: true/false # Bool value to switch role on off.

list_name:
  - schedule name
  - job template name
  - description 
  - rrule

Step 3: Call the role from your playbook.

# Example playbook

## varible definition in group_vars/*.yml
cisco_to_junos:
  - "{{ cisco_job_template[0] }}"
  - "{{ junos_job_template[0] }}"

junos_to_fortios:
  - "{{ junos_job_template[0] }}"
  -  "{{ fortios_job_template[0] }}"

fortios_to_f5:
  -  "{{ fortios_job_template[0] }}"
  -  "{{ f5_job_template[0] }}"
  
##

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
        workflow: "{{ workflow_template_build }}"
        workflow_name: "{{ appc_config_backup_workflow }}"
        nodes_linking_list: 
          - "{{ cisco_to_junos }}"
          - "{{ junos_to_fortios }}"
          - "{{ fortios_to_f5 }}"
    !
    !