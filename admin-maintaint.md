## MySQL Configuration

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


### Edit Configuration (if needed)

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Common configuration parameters:
- `bind-address`: Controls which network interfaces MySQL listens on
- `port`: MySQL port (default: 3306)
- `max_connections`: Maximum number of simultaneous connections
- `innodb_buffer_pool_size`: Memory allocated for InnoDB tables

After making changes, restart MySQL:

```bash
sudo systemctl restart mysql
```

### Service Management Commands

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