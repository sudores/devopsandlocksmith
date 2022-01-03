---        
title: "Grant permissions to user on db"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 
```sql
GRANT <permission_type> ON database.table TO '<username>'@'<host>';
```

Grant all permission on all databases to user from any host
```sql
GRANT all on *.* to '<user>'@'%';
```
