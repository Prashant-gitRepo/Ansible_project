### 1. 🛠️ Install Ansible on Your Control Node (e.g., Amazon Linux EC2)

```bash
sudo yum update -y
sudo yum install -y ansible
ansible --version
```

---

### 2. 🔑 Copy your key pair in main server
Create a `keys` directory and paste your private key:

```bash
mkdir keys
vi keys/ansible_key.pem
# Paste your downloaded private key here
chmod 400 keys/ansible_key.pem
```

---

### 3. 🧾 Update the host file or create if not present

```bash
sudo vi /etc/ansible/hosts
```

Add the following:

```ini
[servers]
server_1 ansible_host=<paste ec2 public ip>
server_2 ansible_host=<paste ec2 public ip>

[all:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=/full/path/to/keys/ansible_key.pem
```

---

### 4. ✅ Check Connection

Run the following to test SSH and Ansible:

```bash
ansible servers -m ping
```

---


### 5. ▶️ Run the Playbook

```bash
ansible-playbook deploy_webpage.yml
```

---

### 6. 🌐 Check the Deployment

Open your browser and enter the public IP of the target EC2 instance. You should see your custom webpage.
