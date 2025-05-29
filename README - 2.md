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

### 3. 🧾 Update the Ansible Hosts File (or create it if it doesn't exist)
Open or create the hosts file:
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
👉 Replace <your-ec2-public-ip-1> and <your-ec2-public-ip-2> with your actual EC2 IP addresses. Ensure the path to your private key is absolute.
---

### 4. ✅ Verify Ansible Can Connect to Your Servers


Run the following command to test SSH and Ansible connectivity:

```bash
ansible servers -m ping
```
You should see output similar to the following:
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


### 5. ▶️ Run the Ansible Playbook
This playbook will deploy your custom webpage to the target EC2 instances. To run it, use the following command:

```bash
ansible-playbook deploy_webpage.yml
```

---

### 6. 🌐 Check the Deployment

Open your browser and navigate to the public IP of the EC2 instance(s).
You should see your custom webpage live.


