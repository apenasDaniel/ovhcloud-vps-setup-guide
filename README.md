


# Comprehensive Guide for Setting Up and Securing an OVHCloud VPS

## Overview
This guide walks through the steps to set up and secure an OVHCloud VPS, covering key security practices such as configuring SSH, enabling a firewall, and monitoring logs.

### What you’ll achieve:
- Secure SSH access using key authentication.
- Harden your VPS by disabling root login and changing the default SSH port.
- Enable and configure UFW firewall.
- Set up basic log monitoring for security.

## Setup Instructions

### 1. Updating the Server
It’s essential to keep the system up to date to protect against known vulnerabilities.

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Adding a Non-Root User
Using a non-root user for daily operations improves security.

```bash
sudo adduser yourusername
sudo usermod -aG sudo yourusername
```

Log in as the new user:

```bash
ssh yourusername@your-vps-ip
```

---

### 3. Configuring SSH Key Authentication
Set up SSH key authentication for secure, passwordless logins.

1. Generate a key pair on your local machine:
   ```bash
   ssh-keygen -t rsa -b 4096
   ```

2. Copy the public key to the VPS:
   ```bash
   ssh-copy-id yourusername@your-vps-ip
   ```

3. Verify login with your SSH key:
   ```bash
   ssh yourusername@your-vps-ip
   ```

---

### 4. Disabling Root Login for SSH
Prevent root from logging in remotely by editing the SSH config.

1. Open the SSH config file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Set the following:
   ```bash
   PermitRootLogin no
   ```

3. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   ```

---

### 5. Changing SSH to a Non-Default Port
To further secure SSH, change the default port (22) to a non-standard one.

1. Open the SSH config file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. Change the port:
   ```bash
   Port 2222  # or any other port you prefer
   ```

3. Allow the new port in the firewall (UFW):
   ```bash
   sudo ufw allow 2222/tcp
   ```

4. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   ```

---

## Security Enhancements

### 6. Enabling UFW Firewall
UFW (Uncomplicated Firewall) helps control which services are allowed to interact with the server.

1. Allow essential ports (SSH, HTTP, HTTPS):
   ```bash
   sudo ufw allow 2222/tcp
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```

2. Enable the firewall:
   ```bash
   sudo ufw enable
   ```

3. Check firewall status:
   ```bash
   sudo ufw status
   ```

---

## Monitoring Logs

### 7. Monitoring SSH Access Logs
It’s important to monitor SSH access attempts, especially for failed login attempts that could indicate brute-force attacks.

1. View SSH access in real time:
   ```bash
   sudo tail -f /var/log/auth.log
   ```

2. Check for failed login attempts:
   ```bash
   sudo grep "Failed password" /var/log/auth.log
   ```

---

### 8. Monitoring Nginx Logs
If you're running a website on your VPS, monitoring Nginx logs helps detect unusual traffic or errors.

1. Access logs:
   ```bash
   sudo tail -f /var/log/nginx/access.log
   ```

2. Error logs:
   ```bash
   sudo tail -f /var/log/nginx/error.log
   ```

---

## Final Recommendations
- Regularly update your server with `sudo apt update && sudo apt upgrade`.
- Consider using tools like Fail2Ban to block repeated failed login attempts.
- Schedule regular checks of your logs to catch any suspicious activity early.

