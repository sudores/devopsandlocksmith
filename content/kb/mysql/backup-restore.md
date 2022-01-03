---        
title: "Create and restore backup mysql"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 

Full db backup
```bash
mysqldump --all-databases --single-transaction --quick --lock-tables=false > full-backup.sql -u root -p
```

Partial db backup
```bash
mysqldump -u [user] -p [database_name] > [filename].sql
```

Restore full db backup 
```bash
mysql -u root -p < full-backup.sql
```

Restore partial backup 
```bash
mysql -u [user] -p [database_name] < [filename].sql
```
