# First Ansible playbook. 
Let's create a simple playbook that installs and starts the Apache web server on a remote host. Here are the steps:

## Step 1: Install Ansible
Make sure you have Ansible installed on your local machine. You can follow the official Ansible installation guide for your specific operating system: https://docs.ansible.com/ansible/latest/installation_guide/index.html

## Step 2: Set up Inventory
Create an inventory file (e.g., `hosts`) that lists the target hosts where you want to install Apache. For this example, we'll assume you have a single host with the IP address `your_server_ip`.

```ini
[web_servers]
your_server_ip
```

## Step 3: Create the Playbook
Create a YAML file (e.g., `install_apache.yml`) for your playbook with the following content:

```yaml
---
- name: Install and Start Apache
  hosts: web_servers
  become: true  # To run tasks with administrative privileges (sudo)

  tasks:
    - name: Update package cache
      apt:
        update_cache: yes
      when: ansible_os_family == 'Debian'  # Use 'yum' instead of 'apt' for RHEL/CentOS based systems

    - name: Install Apache (Debian/Ubuntu)
      apt:
        name: apache2
        state: present
      when: ansible_os_family == 'Debian'

    - name: Install Apache (RHEL/CentOS)
      yum:
        name: httpd
        state: present
      when: ansible_os_family == 'RedHat'

    - name: Start Apache service
      service:
        name: apache2
        state: started
      when: ansible_os_family == 'Debian'

    - name: Start Apache service
      service:
        name: httpd
        state: started
      when: ansible_os_family == 'RedHat'
```

This playbook performs the following tasks:
1. Updates the package cache (Debian/Ubuntu only).
2. Installs Apache using the appropriate package manager based on the OS family.
3. Starts the Apache service.

## Step 4: Run the Playbook
To execute the playbook, use the `ansible-playbook` command:

```bash
ansible-playbook -i hosts install_apache.yml
```

Ansible will connect to the remote host, execute the tasks, and install Apache. If everything goes well, you should have Apache running on your remote server.

Please note that this is a basic example. In real-world scenarios, you may need to handle more complex configurations and handle different types of systems, but this should give you a good starting point for creating your first Ansible playbook. Happy automating!