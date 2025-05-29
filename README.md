# Ansible Nginx Role Project

This project demonstrates how to use **Ansible roles** to provision a remote server and install **Nginx** automatically. It's a great starter project for learning about Ansible best practices and reusable playbook structure.


## 1 Project Structure

ansible_nginx_role/
├── inventory.ini
├── site.yml
└── roles/
└── nginx/
├── tasks/
│ └── main.yml
├── handlers/
│ └── main.yml
├── defaults/
│ └── main.yml
└── templates/
└── nginx.conf.j2

## 2. Set Up Your SSH Key
Ensure your SSH private key can access your server and is not password protected.

```chmod 400 ~/.ssh/your-key.pem```

## 3. Edit inventory.ini
Update with your server's IP and SSH username:

ini
[webservers]
< server IP > ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/your-key.pem

## 4. Test SSH Connection
```ssh -i ~/.ssh/your-key.pem ubuntu@your-IP```
## 5. Run the Playbook
```ansible-playbook -i inventory.ini site.yml```
## Example site.yml

- name: Set up Nginx web server
  hosts: webservers
  become: yes
  roles:
    - nginx
✍️ Example Role Task (roles/nginx/tasks/main.yml)


- name: Install Nginx
  apt:
    name: nginx
    state: present
    update_cache: yes

- name: Copy Nginx config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Restart Nginx
🔁 Handler Example (roles/nginx/handlers/main.yml)


- name: Restart Nginx
  service:
    name: nginx
    state: restarted 
🧰 Requirements
Python 3.x

Ansible 2.9+

A remote Linux server (Ubuntu/Debian recommended)

SSH key-based authentication set up

## 🧼 Cleanup (Optional)
To remove Nginx from the server:

ansible webservers -i inventory.ini -m apt -a "name=nginx state=absent" --become
