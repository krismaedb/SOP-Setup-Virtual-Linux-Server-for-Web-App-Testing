# SOP: Setup Virtual Linux Server for Web App Testing
**Version 1.0 | Date: Nov 20, 2025 | Author: Kristine Bagsican**

---

## Purpose
This SOP provides step-by-step instructions to set up a Virtual Linux Server (Ubuntu) on VMware for web application testing.  
The server will be configured with a **LAMP stack (Linux, Apache, MySQL, PHP)** to ensure it is test-ready for web applications.

---

## Scope
This SOP applies to IT students, lab personnel, and instructors responsible for deploying and testing virtual Linux servers for web applications.  
Objectives:  
- Reproducible VM setup  
- Web application test-ready environment  
- Functional verification of services and network connectivity  

---

## Accountability Matrix

| Role | Responsibility |
|------|----------------|
| Student / SOP Author | Create VM, install OS & software, document steps |
| Instructor / Reviewer | Review and approve SOP |
| Lab Admin | Maintain host hardware & VMware infrastructure |

---

## Definitions
- **VM** → Virtual Machine  
- **LAMP** → Linux, Apache, MySQL, PHP  
- **DHCP** → Dynamic Host Configuration Protocol  
- **ISO** → Disk image for OS installation  

---

## Reversion Table

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Nov 20, 2025 | Kristine Bagsican | Initial SOP creation |

---

## Procedure Steps

### Step 1: Pre-creation Planning
- Determine VM purpose: Web Application Testing  
- Resource requirements: CPU: 2 cores, RAM: 2–4 GB, Disk: 20–30 GB  
- Network: DHCP or static IP  

### Step 2: Virtual Machine Creation
1. Open **VMware vSphere Client** → connect to vCenter.  
2. Navigate to host/cluster → Right-click → New Virtual Machine.  
3. Set VM name: `Ubuntu-WebTest-01`.  
4. Guest OS: Ubuntu Server 24.04 (64-bit).  
5. CPU: 2 cores, RAM: 2–4 GB, Disk: 20 GB (LVM).  
6. Attach Ubuntu Server ISO.  
7. Network: VM Network → connected.  

### Step 3: Guest OS Installation
1. Power on VM → boot from ISO.  
2. Select language, region, keyboard layout.  
3. Storage configuration: Use entire disk with LVM logical volumes: `/boot` 2 GB, `/` remaining space.  
4. Create user account & password.  
5. Reboot after installation.  

### Step 4: Server Setup and LAMP Stack Installation

#### 4.1 Update System and Configure Server
After the OS installation, update the system and configure the server:
```bash
sudo apt update && sudo apt upgrade -y
sudo hostnamectl set-hostname ubuntu@admin
ip a
ip r
sudo apt install net-tools git curl wget unzip open-vm-tools -y
```

#### 4.2 Install Apache Web Server
```bash
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

#### 4.3 Install MySQL Database Server
```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```

#### 4.4 Install PHP
```bash
sudo apt install php libapache2-mod-php php-mysql -y
sudo systemctl restart apache2
```

#### 4.5 Verify PHP Installation
Create a PHP info test page:
```bash
cd /var/www/html
sudo vim info.php
```

Add the following content to `info.php`:
```php
<?php
phpinfo();
?>
```

Save and exit the editor.

Open a web browser and navigate to `http://10.128.200.177/info.php` to verify the PHP info page loads successfully.

#### 4.6 Verify Services and Network Connectivity
Confirm all services are running and network is functional:
```bash
sudo systemctl status apache2
sudo systemctl status mysql
ping -c 4 10.128.200.1
ping -c 4 8.8.8.8
```

---

## Troubleshooting

### Apache won't start
```bash
sudo systemctl status apache2
sudo journalctl -u apache2 -n 50
```

### MySQL connection issues
```bash
sudo systemctl status mysql
sudo mysql -u root -p
```

### PHP not displaying
```bash
php -v
sudo systemctl restart apache2
```

---

## Approval Table
| Name | Role | Signature | Date |
|---------|------|--------|---------|
| Instructor Name | Reviewer | __________ | Nov 20, 2025 |

---

## References
- Ubuntu Server 24.04 LTS Installation Guide - https://ubuntu.com/server/docs
- VMware vSphere Documentation - https://docs.vmware.com
- Apache HTTP Server Documentation - https://httpd.apache.org/docs/
- MySQL Documentation - https://dev.mysql.com/doc/
- PHP Manual - https://www.php.net/manual/
