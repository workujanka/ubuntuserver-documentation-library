# Ansible Basics (Automation & Configuration Management)
# ------------------------------------------------------
# This lesson introduces Ansible for automating Linux systems:
#   - inventory files
#   - ad‑hoc commands
#   - playbooks
#   - modules
#   - roles
#   - SSH-based automation
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. What Is Ansible?
# ---------------------------------------------------------------
# Ansible is an automation tool used for:
#   - configuration management
#   - software installation
#   - server provisioning
#   - orchestration
#
# Features:
#   - agentless (uses SSH)
#   - YAML-based playbooks
#   - idempotent (safe to run repeatedly)

# ---------------------------------------------------------------
# 2. Install Ansible (Ubuntu)
# ---------------------------------------------------------------

sudo apt update
sudo apt install ansible -y

## Check version
ansible --version

# ---------------------------------------------------------------
# 3. Inventory File (Hosts)
# ---------------------------------------------------------------
# Ansible needs a list of servers to manage.

## Default inventory
cat /etc/ansible/hosts

## Example custom inventory:
# [web]
# 192.168.1.10
# 192.168.1.11
#
# [db]
# 192.168.1.20

## Create your own inventory
nano inventory.ini

# ---------------------------------------------------------------
# 4. Test Connectivity
# ---------------------------------------------------------------

## Ping all hosts
ansible all -i inventory.ini -m ping

## Ping only web group
ansible web -i inventory.ini -m ping

# ---------------------------------------------------------------
# 5. Ad‑Hoc Commands
# ---------------------------------------------------------------

## Run command on all hosts
ansible all -i inventory.ini -m shell -a "uptime"

## Install package
ansible all -i inventory.ini -m apt -a "name=nginx state=present" --become

## Copy file
ansible all -i inventory.ini -m copy -a "src=app.conf dest=/etc/app.conf"

## Restart service
ansible all -i inventory.ini -m service -a "name=nginx state=restarted" --become

# ---------------------------------------------------------------
# 6. Ansible Playbooks
# ---------------------------------------------------------------
# Playbooks define tasks in YAML format.

## Example playbook:
nano install_nginx.yml

# ---
# - name: Install Nginx on web servers
#   hosts: web
#   become: yes
#   tasks:
#     - name: Install nginx
#       apt:
#         name: nginx
#         state: present
#
#     - name: Start nginx
#       service:
#         name: nginx
#         state: started

## Run playbook
ansible-playbook -i inventory.ini install_nginx.yml

# ---------------------------------------------------------------
# 7. Variables
# ---------------------------------------------------------------

## Example:
# vars:
#   app_port: 8080
#
# tasks:
#   - name: Print variable
#     debug:
#       msg: "App running on port {{ app_port }}"

## External variable file
nano vars.yml
# app_port: 9090

## Use it:
ansible-playbook -i inventory.ini play.yml -e @vars.yml

# ---------------------------------------------------------------
# 8. Templates (Jinja2)
# ---------------------------------------------------------------

## Template file:
nano app.conf.j2
# server {
#   listen {{ app_port }};
# }

## Use template module:
# - name: Deploy config
#   template:
#     src: app.conf.j2
#     dest: /etc/app/app.conf

# ---------------------------------------------------------------
# 9. Handlers (Run Only When Needed)
# ---------------------------------------------------------------

## Example:
# tasks:
#   - name: Update config
#     template:
#       src: app.conf.j2
#       dest: /etc/app/app.conf
#     notify: restart app
#
# handlers:
#   - name: restart app
#     service:
#       name: app
#       state: restarted

# ---------------------------------------------------------------
# 10. Roles (Reusable Automation)
# ---------------------------------------------------------------

## Create role structure
ansible-galaxy init webserver

## Directory structure:
# webserver/
#   tasks/
#   handlers/
#   templates/
#   vars/
#   defaults/
#   files/

## Use role in playbook:
# - hosts: web
#   roles:
#     - webserver

# ---------------------------------------------------------------
# 11. Ansible Vault (Encrypt Secrets)
# ---------------------------------------------------------------

## Create encrypted file
ansible-vault create secrets.yml

## Edit encrypted file
ansible-vault edit secrets.yml

## Run playbook with vault
ansible-playbook play.yml --ask-vault-pass

# ---------------------------------------------------------------
# 12. Useful Modules
# ---------------------------------------------------------------

## apt → install packages
## service → manage services
## copy → copy files
## template → Jinja2 templates
## user → manage users
## file → manage permissions
## git → clone repositories
## cron → schedule tasks

# ---------------------------------------------------------------
# 13. Practical Examples
# ---------------------------------------------------------------

## 1. Install Docker on all servers
ansible all -i inventory.ini -m apt -a "name=docker.io state=present" --become

## 2. Deploy website using template
ansible-playbook -i inventory.ini deploy_site.yml

## 3. Create users
ansible all -i inventory.ini -m user -a "name=dev state=present" --become

## 4. Restart services after config change
ansible-playbook -i inventory.ini update_config.yml

# ---------------------------------------------------------------
# 14. Summary
# ---------------------------------------------------------------
# - Inventory → list of servers
# - Ad‑hoc commands → quick tasks
# - Playbooks → automation scripts
# - Modules → building blocks
# - Templates → dynamic configs
# - Roles → reusable automation
# - Vault → encrypt secrets
#
# Next lesson:
# → Linux Cloud Fundamentals (AWS, Azure, GCP basics)
