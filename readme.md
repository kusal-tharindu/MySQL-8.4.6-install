# How to Install MySQL 8.4.6 on Ubuntu Server and Secure It

**Author:** Kusal  
**Date:** December 10, 2025  
**OS:** Ubuntu 22.04.5 LTS (Jammy)  
**MySQL Version:** 8.4.6 Community Server

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation Method](#installation-method)
3. [Step-by-Step Installation](#step-by-step-installation)
4. [Secure MySQL Installation](#secure-mysql-installation)
5. [Verify Installation](#verify-installation)
6. [Useful Commands](#useful-commands)
7. [Important File Locations](#important-file-locations)
8. [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before starting the installation, ensure your system is up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

Check your Ubuntu version:

```bash
lsb_release -a
```

---

## Installation Method

This guide uses the **Direct Download Method** from Oracle's official website. This method avoids GPG key expiration issues that can occur with the MySQL APT repository.

---

## Step-by-Step Installation

### Step 1: Install Required Dependencies

```bash
sudo apt install -y libaio1 libmecab2 libnuma1
```

### Step 2: Download MySQL 8.4.6 Bundle

Navigate to a temporary directory and download the MySQL bundle:

```bash
cd /tmp
wget https://dev.mysql.com/get/Downloads/MySQL-8.4/mysql-server_8.4.6-1ubuntu22.04_amd64.deb-bundle.tar
```

> **Note:** File size is approximately 490MB

### Step 3: Extract the Bundle

```bash
tar -xvf mysql-server_8.4.6-1ubuntu22.04_amd64.deb-bundle.tar
```

This extracts the following packages:

| Package | Description |
|---------|-------------|
| mysql-common | Common files for MySQL |
| mysql-community-client-plugins | Client plugins |
| mysql-community-client-core | Core client binaries |
| mysql-community-client | MySQL client |
| mysql-client | Client meta package |
| mysql-community-server-core | Core server binaries |
| mysql-community-server | MySQL server |
| mysql-server | Server meta package |

### Step 4: Install Packages in Order

The packages must be installed in a specific order due to dependencies:

```bash
# 1. Common package
sudo dpkg -i mysql-common_8.4.6-1ubuntu22.04_amd64.deb

# 2. Client plugins
sudo dpkg -i mysql-community-client-plugins_8.4.6-1ubuntu22.04_amd64.deb

# 3. Client core
sudo dpkg -i mysql-community-client-core_8.4.6-1ubuntu22.04_amd64.deb

# 4. Community client
sudo dpkg -i mysql-community-client_8.4.6-1ubuntu22.04_amd64.deb

# 5. Client meta package
sudo dpkg -i mysql-client_8.4.6-1ubuntu22.04_amd64.deb

# 6. Server core
sudo dpkg -i mysql-community-server-core_8.4.6-1ubuntu22.04_amd64.deb

# 7. Community server
# This will prompt you to set a root password for MySQL. Enter a strong password and remember it.
sudo dpkg -i mysql-community-server_8.4.6-1ubuntu22.04_amd64.deb

# 8. Server meta package
sudo dpkg -i mysql-server_8.4.6-1ubuntu22.04_amd64.deb
```

> **Tip:** You can also install all packages at once (order handled automatically):
> ```bash
> sudo dpkg -i mysql-common_8.4.6-1ubuntu22.04_amd64.deb \
>            mysql-community-client-plugins_8.4.6-1ubuntu22.04_amd64.deb \
>            mysql-community-client-core_8.4.6-1ubuntu22.04_amd64.deb \
>            mysql-community-client_8.4.6-1ubuntu22.04_amd64.deb \
>            mysql-client_8.4.6-1ubuntu22.04_amd64.deb \
>            mysql-community-server-core_8.4.6-1ubuntu22.04_amd64.deb \
>            mysql-community-server_8.4.6-1ubuntu22.04_amd64.deb \
>            mysql-server_8.4.6-1ubuntu22.04_amd64.deb
> ```

---
## Verify Installation

### Check MySQL Service Status

```bash
sudo systemctl status mysql
```
Enable MySQL to start automatically on system boot:

```bash
sudo systemctl enable mysql
```
---

## Secure MySQL Installation

After installation, secure your MySQL server:

```bash
sudo mysql_secure_installation
```

### Security Configuration Options

| Prompt | Recommended Response | Description |
|--------|---------------------|-------------|
| VALIDATE PASSWORD component | `y` | Enables password strength validation |
| Password validation policy | `0`, `1`, or `2` | LOW=8 chars, MEDIUM=mixed+special, STRONG=dictionary check |
| Set/Change root password | `y` | Set a strong root password |
| Remove anonymous users | `y` | Removes anonymous user accounts |
| Disallow root login remotely | `y` | Prevents root access from remote hosts |
| Remove test database | `y` | Removes the test database |
| Reload privilege tables | `y` | Applies changes immediately |

---

Expected output should show `Active: active (running)`.

### Verify MySQL Version

```bash
mysql --version
```

Expected output:
```
mysql  Ver 8.4.6 for Linux on x86_64 (MySQL Community Server - GPL)
```

### Test MySQL Login

```bash
mysql -u root -p
```

Enter your root password to access the MySQL prompt.

---

## Useful Commands

### Service Management

```bash
# Start MySQL
sudo systemctl start mysql

# Stop MySQL
sudo systemctl stop mysql

# Restart MySQL
sudo systemctl restart mysql

# Check status
sudo systemctl status mysql

# Enable on boot
sudo systemctl enable mysql

# Disable on boot
sudo systemctl disable mysql
```

### MySQL Client Commands

```bash
# Login as root
mysql -u root -p

# Login to specific database
mysql -u root -p database_name

# Execute SQL from command line
mysql -u root -p -e "SHOW DATABASES;"
```

---

## Important File Locations

| Item | Path |
|------|------|
| Configuration file | `/etc/mysql/mysql.cnf` |
| Data directory | `/var/lib/mysql` |
| Error log | `/var/log/mysql/error.log` |
| Socket file | `/var/run/mysqld/mysqld.sock` |
| PID file | `/var/run/mysqld/mysqld.pid` |
| Systemd service | `/lib/systemd/system/mysql.service` |

---
