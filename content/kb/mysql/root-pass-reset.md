---        
title: "Reset root password mysql 5.7"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 

1. Stop mysql:
```bash  
systemctl stop mysqld
```

2. Set the mySQL environment option 
```bash  
systemctl set-environment MYSQLD_OPTS="--skip-grant-tables"
```

3. Start mysql usig the options you just set
```bash  
systemctl start mysqld
```

4. Login as root
```bash  
mysql -u root
```

5. Update the root user password with these mysql commands
```sql 
UPDATE mysql.user SET authentication_string = PASSWORD('MyNewPassword') WHERE User = 'root' AND Host = 'localhost';
FLUSH PRIVILEGES;
QUIT
```
*** Edit ***
As mentioned my shokulei in the comments, for 5.7.6 and later, you should use 
```sql 
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```
Or you'll get a warning

6. Stop mysql
```bash  
systemctl stop mysqld
```

7. Unset the mySQL envitroment option so it starts normally next time
```bash  
systemctl unset-environment MYSQLD_OPTS
```
8. Start mysql normally:
```bash  
systemctl start mysqld
```

9. Try to login using your new password:
```bash  
mysql -u root -p
```

10. If necessary set password (mostly if error is "You have change your user password using alter statement")
```sql
 SET PASSWORD = PASSWORD('your_new_password');
```

