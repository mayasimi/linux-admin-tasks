I was given an assignment to set up and manage a Linux server for my company. This included tasks such as user and role management, system monitoring, application management, and implementing security measures. Below is a detailed breakdown of the steps I took to complete this assignment.

# Comprehensive Guide to Linux Server Administration

1. User and Roles Management

Your company recently hired five new developers who need access to the development server. Follow these steps to set up their accounts and permissions:

Step 1: Create User Accounts**
Create user accounts for the new developers:
```bash
for user in dev1 dev2 dev3 dev4 dev5; do
  sudo adduser $user
  sudo passwd $user
done
```

Step 2: Create and Add Users to the Developers Group
```bash
sudo groupadd developers
for user in dev1 dev2 dev3 dev4 dev5; do
  sudo usermod -aG developers $user
done
```

Step 3: Set File Permissions
Give read and execute permissions to `/var/www/project` but restrict modifications:
```bash
sudo chown -R root:developers /var/www/project
sudo chmod -R 750 /var/www/project
```

Step 4: Restrict SSH Access**
For `dev4` and `dev5`, restrict SSH access:
```bash
echo 'DenyUsers dev4 dev5' | sudo tee -a /etc/ssh/sshd_config
sudo systemctl restart sshd
```
 2. System Monitoring & Performance Analysis

Step 1: Identify Resource-Consuming Processes**
```bash
top
```
If a process is consuming too many resources, use:
```bash
ps aux --sort=-%cpu | head -10
ps aux --sort=-%mem | head -10
```
To kill a process:
```bash
sudo kill -9 <PID>
```

Step 2: Check Disk Usage**
```bash
df -h
du -sh /var/log
```
If logs consume too much space, clean them:
```bash
sudo journalctl --vacuum-time=7d
```

Step 3: Monitor Real-Time System Logs
```bash
tail -f /var/log/syslog
```

 3. Application Management

Step 1: Install Nginx
```bash
sudo apt update
sudo apt install nginx -y
```

Step 2: Enable Nginx to Start on Boot
```bash
sudo systemctl enable nginx
```

Step 3: Check Nginx Status**
```bash
sudo systemctl status nginx
```

Step 4: Restart Nginx if it Crashes**
Create a systemd watchdog service:
```bash
sudo systemctl restart nginx
```
To automate recovery:
```bash
sudo systemctl edit nginx.service
```
Add:
```
Restart=always
```

4. Networking and Security

Step 1: Configure Firewall**
```bash
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw allow http
sudo ufw enable
```

Step 2: Check Open Ports**
```bash
sudo netstat -tulnp
```

Step 3: Set Up SSH Key-Based Authentication**![IMG_6701](https://github.com/user-attachments/assets/c973a410-8234-4622-b919-b6dc8b410713)
![IMG_6700](https://github.com/user-attachments/assets/4f2e46f1-d638-4959-b89e-c388b69bce88)
![IMG_6699](https://github.com/user-attachments/assets/e6703471-ee46-4070-a341-3a3e91764734)
![IMG_6697](https://github.com/user-attachments/assets/ad91684a-e0e3-4e52-a462-9e923c3864f8)
![IMG_6696](https://github.com/user-attachments/assets/fd1cacab-579e-4233-be7d-171584cb9fd6)
![IMG_6695](https://github.com/user-attachments/assets/405819ad-3ef0-40fd-a9ad-868e636ef5e4)

Generate SSH Key:
```bash
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
```
Copy the key to the server:

ssh-copy-id user@your-server-ip

Disable password authentication:

echo 'PasswordAuthentication no' | sudo tee -a /etc/ssh/sshd_config
sudo systemctl restart sshd
