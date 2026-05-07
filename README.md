• ansible-galaxy list shows both roles
• ansible-playbook site.yml runs without errors
# 🚀 Ansible Lab — Roles & Galaxy (Beginner Friendly)

## 🧠 Goal
In this lab we will:
- Build a reusable Nginx role using `ansible-galaxy`
- Use a community MySQL role from Galaxy
- Run both roles using a single playbook (`site.yml`)
- Apply everything on a local VM (192.168.148.132)

---

## 🖥️ Environment

We are using ONE VM:
- IP: `192.168.148.132`
- Acts as:
  - web1 (nginx)
  - db1 (mysql)

---

## 📁 Project Structure


~/ansible-lab/
├── ansible.cfg
├── inventory/
│ └── hosts.yml
├── requirements.yml
├── site.yml
└── roles/
└── nginx/
├── tasks/
├── handlers/
├── templates/
├── defaults/
└── meta/


