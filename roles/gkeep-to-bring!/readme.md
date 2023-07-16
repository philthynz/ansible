## Overview
This role was created as a temporary workaround for Google's drop of 3rd party note app support. Which meant the Bring! App would no longer work with Google Home/Assistant, see post [here](https://www.getbring.com/blog-posts/google-assistant-no-more-support-for-third-party-list-apps).

The role copies note items from Google Keep to Bring! and then deletes the same items in Keep. Almost like a one-way sync. The intention being that you use your Google Home or Assistant to add items to Google's shopping list (keep) and then run this role on a regular basis to copy them to Bring! Hopefully Google add support for 3rd party note apps again soon.

This role spins up a docker container that works with the required python packages, and run's some commands on it to achieve the automation. A combination of tools are used. A big thanks to these developers for making this possible:
 - https://github.com/Nekmo/gkeep
 - https://pypi.org/project/python-bring-api/

## Requirements
All you need is a host with Docker installed.

## Using the role
1. Install the role with `ansible-galaxy install -r requirements.yml`
2. Optional - Update [ansible.cfg](../../playbook-examples/ansible.cfg) with where the role was installed and run `export ANSIBLE_CONFIG=ansible.cfg` to use it.
3. Edit the [example playbook](../../playbook-examples/gkeep-to-bring!.yml) variables with the values you require, some of the variables have defaults. You can pass them in the playbook file like in the example, via Ansible Vault or CLI. 

## Run Playbook Example
```
ansible-playbook gkeep-to-bring!.yml
```

## Variables
| Variable Name | Description         | Default |
|----------|--------------------------|---------|
| container_name | Defines the container name that Docker spins up | gkeep-to-bring |
| gkeep_note_name | The name of the note in Google Keep that you want to get the items from | Shopping |
| bring_list_name | The name of the list in Bring! that you want to get the items from | Shopping |
| gmail_username | Your Gmail username/email |  |
| gmail_app_password | Your Gmail App Password (Not login password) |  |
| bring_email  | Your Bing! email/login |  |
| bring_password | Your Bring! password |  |
