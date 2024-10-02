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
example-vars.yml

## playbook 
example-playbook.yml
