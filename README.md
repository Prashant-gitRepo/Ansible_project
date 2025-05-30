# Ansible Webpage Deployment on Other servers Guide

This guide will walk you through setting up an Ansible environment to deploy a custom HTML webpage to target EC2 instances using an Ansible playbook.

---

### 🚀 Launch EC2 Instances for Ansible Setup and Testing

Before starting with Ansible setup, you'll need to launch three EC2 instances:

1. **Main Ansible Server** – This instance will host Ansible and control the other instances.
2. **Test EC2 Instances** – These instances will be the targets for your Ansible playbooks.

---

### Step 1: Launch the Main Ansible Server

Follow these steps to launch the main Ansible server:

1. Go to the [AWS Management Console](https://aws.amazon.com/console/) and navigate to EC2.
2. Click **Launch Instance** and select **Amazon Linux 2 AMI**.
3. Choose an instance type (e.g., **t2.micro** for free-tier eligible users).
4. Follow the prompts and create a new key pair, or use an existing one.
5. Set security groups to allow SSH (port 22) from your IP address.
6. Launch the instance and save the **public IP**.

---

### Step 2: Launch Two EC2 Instances for Testing

Repeat the process for two additional instances:

1. Select the same **Amazon Linux 2 AMI** for both.
2. Choose the same instance type.
3. Set up the security groups to allow SSH (port 22) and HTTP (port 80) access.
4. Save the **public IPs** of the test instances.

---

### Step 3: 🛠️ Install Git and pull repository
1. Install git and clone the repository files (index and playbook file)
```bash
sudo yum update -y
sudo yum install git -y
```

2. Clone the repository files (index and playbook file)
```bash
git clone https://github.com/Prashant-gitRepo/Ansible_project.git
```

3. Check files
```bash
cd Ansible_project
ls
```

---

### Step 4: 🛠️ Install Ansible on Your Main server (e.g., Amazon Linux EC2)
Install Ansible:
```bash
sudo yum install -y ansible
ansible --version
```

---

### Step 5: 🔑 Copy your SSH key pair to the main server
1. Create a directory for your key:
```bash
sudo mkdir keys
sudo vi keys/ansible_key.pem
```

2. Paste your private .pem key and save (used to connect to target EC2 instances):

3. Set correct permissions:
```bash
sudo chmod 400 keys/ansible_key.pem
```

---

### Step 6: 🧾 Inventory Configuration
Edit Ansible's inventory file:
```bash
sudo vi /etc/ansible/hosts
```

Add your server IP addresses:

```ini
[servers]
server_1 ansible_host=<your-ec2-public-ip-1>
server_2 ansible_host=<your-ec2-public-ip-2>

[all:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=/full/path/to/keys/ansible_key.pem
```
Replace (your-ec2-public-ip-1) and (your-ec2-public-ip-2) with your actual Ec2 Ip Addresses. Ensure the path to your private key is absolute. To check path use pwd command

---

### Step 7:✅ Test Ansible Connection


1. Run the following command to test SSH and Ansible connectivity:

```bash
sudo ansible servers -m ping
```
2. You should see output similar to the following:
```ini
server_1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
```
If you see this output, your connection is successfully established!

---


### Step 8: ▶️ Run the Ansible Playbook
This playbook will deploy your custom webpage to the target EC2 instances. To run it, use the following command:

```bash
sudo ansible-playbook deploy_webpage.yml
```
This will:

Install and start Nginx

Copy index.html to the web root

---

### Step 9: 🌐 Check the Deployment

Open your browser and navigate to the public IP of the EC2 instance(s).
You should see your custom webpage live.

---

## 📁 Project Structure
```
Ansible_project/
├── deploy_webpage.yml      # Ansible playbook to deploy the webpage
├── index.html              # Webpage file to be deployed
└── keys/
    └── ansible_key.pem     # SSH private key (should be .gitignored)
```
---



