# 🚀 Ansible Lab — Roles & Galaxy (Beginner Friendly)

ذذذذذذّّّ
----------------
<img width="1354" height="761" alt="galaxy3" src="https://github.com/user-attachments/assets/04b56309-f55b-4fee-b6ab-b3fb158dea5c" />
<img width="1360" height="768" alt="galaxy2" src="https://github.com/user-attachments/assets/4f318df8-a899-4de4-b82e-d85d393b7dda" />

## 🧠 Goal

In this lab we will:

- Build a reusable Nginx role using `ansible-galaxy`
- Use a community MySQL role from Galaxy
- Run both roles using a single playbook (`site.yml`)
- Apply everything on a local VM (`192.168.148.132`)

---

## 🖥️ Environment

We are using **ONE VM**:

- IP: `192.168.148.132`

Acts as:

- `web1` → Nginx Server
- `db1` → MySQL Server

---

## 📁 Project Structure

```text
~/ansible-lab/
├── ansible.cfg
├── inventory/
│   └── hosts.yml
├── requirements.yml
├── site.yml
└── roles/
    └── nginx/
        ├── tasks/
        ├── handlers/
        ├── templates/
        ├── defaults/
        └── meta/
⚙️ Step 1 — Create Project
mkdir -p ~/ansible-lab
cd ~/ansible-lab

mkdir -p inventory roles
⚙️ Step 2 — Inventory File

📄 inventory/hosts.yml

all:
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.148.132

    dbservers:
      hosts:
        db1:
          ansible_host: 192.168.148.132
⚙️ Step 3 — ansible.cfg

📄 ansible.cfg

[defaults]
inventory = ./inventory/hosts.yml
roles_path = ./roles
host_key_checking = False
⚙️ Step 4 — Create Nginx Role
ansible-galaxy role init roles/nginx
Clean unnecessary folders
rm -rf roles/nginx/files
rm -rf roles/nginx/vars
rm -rf roles/nginx/tests
⚙️ Step 5 — Defaults

📄 roles/nginx/defaults/main.yml

nginx_port: 80
nginx_root: /var/www/html
nginx_worker_connections: 1024
site_title: "My Nginx Site"
⚙️ Step 6 — Template

📄 roles/nginx/templates/nginx.conf.j2

events {
    worker_connections {{ nginx_worker_connections }};
}

http {
    server {
        listen {{ nginx_port }};
        server_name {{ inventory_hostname }};
        root {{ nginx_root }};
        index index.html;
    }
}
⚙️ Step 7 — Tasks

📄 roles/nginx/tasks/main.yml

- name: Install nginx
  apt:
    name: nginx
    state: present
    update_cache: yes

- name: Create web root
  file:
    path: "{{ nginx_root }}"
    state: directory
    owner: www-data
    group: www-data
    mode: '0755'

- name: Deploy nginx config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Restart nginx

- name: Create index page
  copy:
    dest: "{{ nginx_root }}/index.html"
    content: "Hello from {{ inventory_hostname }}"

- name: Start nginx
  service:
    name: nginx
    state: started
    enabled: true
⚙️ Step 8 — Handler

📄 roles/nginx/handlers/main.yml

- name: Restart nginx
  service:
    name: nginx
    state: restarted
⚙️ Step 9 — Galaxy Role

📄 requirements.yml

roles:
  - name: geerlingguy.mysql
    version: "6.3.1"
Install Galaxy Role
ansible-galaxy install -r requirements.yml
⚙️ Step 10 — Site Playbook

📄 site.yml

- name: Web servers setup
  hosts: webservers
  become: true

  roles:
    - role: nginx
      vars:
        nginx_port: 8080

- name: DB servers setup
  hosts: dbservers
  become: true

  vars:
    mysql_root_password: "SecurePass123"

    mysql_databases:
      - name: appdb

    mysql_users:
      - name: appuser
        host: "%"
        password: "AppPass123"
        priv: "appdb.*:ALL"

  roles:
    - geerlingguy.mysql
🚀 Step 11 — Run Playbook
ansible-playbook site.yml
🧪 Step 12 — Test
Nginx Test
ansible webservers -m uri -a "url=http://localhost:8080 status_code=200"
Galaxy Roles Check
ansible-galaxy list
🎯 Final Result

✅ Nginx installed via custom role
✅ MySQL installed via Galaxy role
✅ Both running on same VM
✅ Fully reusable Ansible roles structure


