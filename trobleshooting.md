## Troubleshooting

### Common Issues and Solutions

#### 1. Cannot Connect to MySQL

**Issue:** Connection refused or timeout

**Solutions:**
- Check if MySQL service is running: `sudo systemctl status mysql`
- Verify bind-address in `/etc/mysql/mysql.conf.d/mysqld.cnf`
- Check firewall rules: `sudo ufw status`
- Verify user has remote access privileges: `SELECT user, host FROM mysql.user;`

#### 2. Access Denied for User

**Issue:** Authentication failed

**Solutions:**
- Verify username and password are correct
- Check user host permissions: `SELECT user, host FROM mysql.user WHERE user='username';`
- Ensure user has proper privileges: `SHOW GRANTS FOR 'user'@'host';`
- Try resetting password: `ALTER USER 'user'@'host' IDENTIFIED BY 'new_password';`

#### 3. MySQL Service Won't Start

**Issue:** Service fails to start

**Solutions:**
- Check error logs: `sudo tail -f /var/log/mysql/error.log`
- Verify configuration syntax: `sudo mysqld --validate-config`
- Check disk space: `df -h`
- Check permissions: `sudo ls -la /var/lib/mysql/`

#### 4. Port Already in Use

**Issue:** Port 3306 is already in use

**Solutions:**
- Find process using port: `sudo lsof -i :3306` or `sudo netstat -tlnp | grep 3306`
- Stop conflicting service or change MySQL port in configuration

#### 5. Repository Package Issues

**Issue:** Cannot download or install MySQL repository package

**Solutions:**
- Check internet connectivity
- Verify repository URL is correct
- Try downloading latest repository package from [MySQL Downloads](https://dev.mysql.com/downloads/repo/apt/)
- Remove old repository: `sudo dpkg -r mysql-apt-config`

### Log File Locations

- Error log: `/var/log/mysql/error.log`
- General query log: `/var/log/mysql/mysql.log`
- Slow query log: `/var/log/mysql/mysql-slow.log`
- Binary log: `/var/log/mysql/mysql-bin.*`

### View Recent Logs

```bash
# View error log
sudo tail -f /var/log/mysql/error.log

# View last 50 lines
sudo tail -n 50 /var/log/mysql/error.log
```

### Service Restart Procedures

If you need to restart MySQL:

```bash
# Normal restart
sudo systemctl restart mysql

# If service is stuck, force stop and start
sudo systemctl stop mysql
sudo systemctl start mysql

# Check status after restart
sudo systemctl status mysql
```

### Reset Root Password (if forgotten)

If you forget the root password:

1. Stop MySQL service:
   ```bash
   sudo systemctl stop mysql
   ```

2. Start MySQL in safe mode:
   ```bash
   sudo mysqld_safe --skip-grant-tables &
   ```

3. Connect without password:
   ```bash
   mysql -u root
   ```

4. Reset password:
   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
   FLUSH PRIVILEGES;
   EXIT;
   ```

5. Restart MySQL normally:
   ```bash
   sudo systemctl restart mysql
   ```

---

## Additional Resources

- [MySQL 8.4 Documentation](https://dev.mysql.com/doc/refman/8.4/en/)
- [MySQL Security Best Practices](https://dev.mysql.com/doc/refman/8.4/en/security.html)
- [MySQL Configuration Reference](https://dev.mysql.com/doc/refman/8.4/en/server-configuration.html)

---