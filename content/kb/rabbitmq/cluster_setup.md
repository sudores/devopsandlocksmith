---        
title: "Setup rabbitmq cluster admin user"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 
```bash
rabbitmqctl add_user username pass                        # add user

rabbitmqctl set_user_tags username administrator          # set user tag to admin

rabbitmqctl set_permissions -p / developer ".*" ".*" ".*" # grant root perm-s

rabbitmqctl set_policy ha-all ".*" '{"ha-mode":"all"}'    # set all queues full mirroring
```
