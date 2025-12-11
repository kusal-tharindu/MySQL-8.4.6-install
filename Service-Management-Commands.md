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
mysql -u