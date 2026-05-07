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

<img width="1354" height="761" alt="galaxy3" src="https://github.com/user-attachments/assets/04b56309-f55b-4fee-b6ab-b3fb158dea5c" />
<img width="1360" height="768" alt="galaxy2" src="https://github.com/user-attachments/assets/4f318df8-a899-4de4-b82e-d85d393b7dda" />


