### 1. 🛠️ Install Ansible on Your Main server (e.g., Amazon Linux EC2)

```bash
sudo yum update -y
sudo yum install -y ansible
ansible --version
```

---

### 2. 🔑 Copy your SSH key pair to the main server
Create a `keys` directory and paste your **private SSH key** (used to connect to target EC2 instances):
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
server_1 ansible_host=<your-ec2-public-ip-1>
server_2 ansible_host=<your-ec2-public-ip-2>

[all:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=/full/path/to/keys/ansible_key.pem
```

---

### 4. ✅ Verify Ansible Can Connect to Your Servers


Run the following command to test SSH and Ansible connectivity:

```bash
ansible servers -m ping
```
You should see output similar to the following:
server_1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
If you see this output, your connection is successfully established!

---


### 5. ▶️ Run the Playbook

```bash
ansible-playbook deploy_webpage.yml
```

---

### 6. 🌐 Check the Deployment

Open your browser and enter the public IP of the target EC2 instance. You should see your custom webpage.
