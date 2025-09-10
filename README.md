# 🛠️ Ansible Deployment Setup

**Project**: Ansible Deployment  
**Date**: 17/04/2025  
**Engineer**: Parvez M B  
**Email**: parvez@gallexyintelligentia.com  

---

## 📦 Step 1: Set Up the Virtual Machine

- Create a VM on **GCP** or **AWS**
- OS: **Ubuntu Server**
- Update system:
  ```bash
  apt update && apt upgrade -y
  ```
- Install Docker:
  ```bash
  apt install docker.io -y
  ```

---

## 🐳 Step 2: Create Docker Containers

```bash
docker run -it -d --name ansible-master ubuntu /bin/bash
docker run -it -d --name target1 ubuntu /bin/bash
docker run -it -d --name target2 ubuntu /bin/bash
docker ps  # Verify containers
```

---

## 📦 Step 3: Configure Ansible-Master Container

```bash
docker exec -it ansible-master bash
apt update
apt install python-is-python3 vim iputils-ping openssh-client -y
apt install software-properties-common
add-apt-repository --yes --update ppa:ansible/ansible
apt install ansible
ansible --version
```

---

## 🎯 Step 4: Configure Target Machines

### Target1 Setup

```bash
docker exec -it target1 bash
apt update
apt install vim python-is-python3 iputils-ping
apt-get install openssh-client openssh-server -y
```

### SSH Configuration

```bash
cd /etc/ssh
vi sshd_config
# Change:
PermitRootLogin yes
PasswordAuthentication yes
```

```bash
service ssh start
passwd root  # Set root password
```

---

## 🌐 Step 5: Network & Host Setup

```bash
docker inspect target1  # Get IP (e.g., 172.17.0.3)
docker exec -it ansible-master bash
cd /etc/ansible
vi hosts
# Add:
172.17.0.3
```

---

## 🔐 Step 6: SSH Key Setup

```bash
ssh-keygen
ssh-copy-id root@172.17.0.3
ssh root@172.17.0.3  # Verify passwordless login
```

---

## 📜 Step 7: Create Ansible Playbook

```yaml
# /etc/ansible/playbooknginx.yaml
- hosts: all
  tasks:
    - name: Ensure nginx is installed
      apt:
        name: nginx
        state: latest
```

Run the playbook:

```bash
ansible-playbook playbooknginx.yaml
```

---

## 🔁 Step 8: Repeat for Target2

- Follow same steps as Target1
- Update `/etc/ansible/hosts` with Target2 IP (e.g., `172.17.0.4`)
- Run playbook again

---

✅ **Deployment Verified**  
🎉 Ansible installed and configured across master and targets.
