# 🔐 Secure Ubuntu Server with UFW and Fail2Ban (SSH Hardening on AWS EC2)

This project demonstrates how to secure an Ubuntu server (e.g., AWS EC2) using UFW (Uncomplicated Firewall) and Fail2Ban. These tools help harden SSH access, block brute-force attacks, and reduce unauthorized access attempts.

---

## 🛠 Tools & Technologies

- UFW (Uncomplicated Firewall)
- Fail2Ban
- systemd / auth logs

---

## 🚀 Project Overview

- Installing and configuring UFW firewall to restrict access
- Installing Fail2Ban to ban IPs after failed SSH login attempts
- Optionally changing SSH default port
- Disabling root SSH login and enforcing key-based authentication

---

## 📋 Step-by-Step Guide

### ✅ 1. Update and Install Packages
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install ufw fail2ban -y
```

### ✅ 2. Configure UFW (Firewall)
```
# Allow SSH
sudo ufw allow OpenSSH

# (Optional) Allow web server ports if needed:
sudo ufw allow 80     # HTTP
sudo ufw allow 443    # HTTPS
```

#### Enable UFW
```
sudo ufw enable
sudo ufw status verbose
```

### ✅ 3. Configure Fail2Ban for SSH
```
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

#### Ensure `[sshd]` looks like this:
```
[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 3
bantime = 1h
findtime = 10m
```

#### Restart the service
```
sudo systemctl restart fail2ban
sudo fail2ban-client status sshd
```

## 📸 Example Output

#### UFW Status
```
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
```

#### Fail2Ban Status
```
Status for the jail: sshd
|- Filter
|  |- Currently failed: 2
|  |- Total failed:     8
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 1
   |- Total banned:     3
```

## 📎 Commands Reference
- `sudo ufw allow OpenSSH` – Allow SSH through the firewall
- `sudo ufw enable` – Enable the firewall
- `sudo apt install fail2ban` – Install Fail2Ban
- `sudo systemctl restart fail2ban` – Restart Fail2Ban
- `sudo fail2ban-client status` – View active bans
- `sudo nano /etc/ssh/sshd_config` – Edit SSH settings

## 💡 Real-World Use Cases
- Secure cloud-hosted Linux servers (AWS, DigitalOcean, etc.)
- Protect against automated SSH brute-force attacks
- Add an extra layer of protection for remote management

## 📚 Learning Outcomes
- Understand how to configure a basic firewall using UFW
- Learn to use Fail2Ban to prevent intrusion attempts
- Practice good SSH hardening techniques
- Gain confidence in securing remote cloud infrastructure

## 🧑‍💻 Author
Orven Casido – techwithorven.xyz
