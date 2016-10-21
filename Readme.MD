# Hello-World Ansible Structure Creator

 Following script will create a hello world startup ansible playbook
 
```
#!/usr/bin/env bash

echo 'my_vars/*.yml' > .gitignore

echo '[defaults]' > ansible.cfg
echo 'hostfile=inventories/dev' >> ansible.cfg
echo "remote_user=${USER}" >> ansible.cfg
echo 'host_key_checking=false' >> ansible.cfg
echo 'log_path=/tmp/ansible.log' >> ansible.cfg


mkdir -p inventories/dev/group_vars
mkdir -p inventories/dev/group_vars/localsSecure
mkdir -p inventories/dev/host_vars
mkdir -p library
mkdir -p filter_plugins
mkdir -p roles/common

mkdir -p roles/common/tasks  # create main.yml for each role sub directory except templates and files
mkdir -p roles/common/handlers
mkdir -p roles/common/templates
mkdir -p roles/common/files
mkdir -p roles/common/vars
mkdir -p roles/common/defaults
mkdir -p roles/common/meta


echo '#!/usr/bin/env ansible-playbook' > site.yml
echo '---' >> site.yml
echo '# file: site.yml' >> site.yml
echo '- include: hello-world.yml' >> site.yml
 
echo '#!/usr/bin/env ansible-playbook' > hello-world.yml
echo '---' >> hello-world.yml
echo '# file: hello-world.yml' >> hello-world.yml
echo '- hosts: locals' >> hello-world.yml
echo '  roles:' >> hello-world.yml
echo '    - common' >> hello-world.yml

chmod 700 site.yml
chmod 700 hello-world.yml

echo '---' > roles/common/tasks/main.yml
echo '# file: roles/common/tasks/main.yml' >> roles/common/tasks/main.yml
echo '- name: Hello World' >> roles/common/tasks/main.yml
echo '  command: "echo Hello World plainText: {{ thisIsClearText }}, secureText: {{ somevar }}"' >> roles/common/tasks/main.yml

echo '[locals]' > inventories/dev/hosts
echo 'localhost' >> inventories/dev/hosts
echo '[localsSecure]' >> inventories/dev/hosts
echo 'localhost' >> inventories/dev/hosts

echo '---' > inventories/dev/group_vars/locals
echo '# locals properties' >> inventories/dev/group_vars/locals
echo 'ansible_connection: local' >> inventories/dev/group_vars/locals

echo 'inventories/dev/group_vars/localsSecure/vault # use ansible-vault to create it and set "vault_*" variables'
echo '---' > inventories/dev/group_vars/localsSecure/vars
echo '# locals properties' >> inventories/dev/group_vars/localsSecure/vars
echo 'thisIsClearText: clear' >> inventories/dev/group_vars/localsSecure/vars
echo 'somevar: "{{ vault_somevar }}"' >> inventories/dev/group_vars/localsSecure/vars

ansible locals -m ping
ansible locals -a 'hostname'
```
