---        
title: "Build Smiddle multiscript gadjet"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 

# Setup multiscript gadget for SmiddleOmniChannel

## Multiscript gadget 
### Multiscript gadget build instruction
- clone git repository to your folder
- open terminal and go to your folder and then enter command `npm install`
- change config variables (URL) in public/index.html
    ```html
<!DOCTYPE html>
     <html lang="en">
       <head>
         <meta charset="utf-8" />
         <meta name="viewport" content="width=device-width, initial-scale=1" />
         <title>React App</title>
         <script>
           var LOGIN_URL = "https://API_GATEWAY_URL/SmiddleAuthorizationServer/sas/auth";
           var GET_SESSION_URL = "https://API_GATEWAY_URL/MessengerGateway/smc/chat/message/session";
           var FILE_STORAGE_URL = "https://API_GATEWAY_URL/SmiddleFileStorage/sfs/files";
           var ADM_CURRENT_USER_URL = "https://API_GATEWAY_URL/SmiddleManager/adm/management/users/current";
           var WS_URL = "wss://API_GATEWAY_URL/SocialMinerConnector/webSocket/751";
           var EXECUTOR_IFRAME_URL = "https://API_GATEWAY_URL/executor/?ani=&scriptId=<SCRIPT_ID>&campaignName=&continueScript=true&lng=en";
           var EXPIRATION_LOGIN_TIMEOUT_IN_HOURS = 8;
         </script>
       </head>
       <body>
         <div id="sm-f-chats"></div>
       </body>
     </html>

    ```
- in variable "EXECUTOR_IFRAME_URL" you should set only: 'ani' or 'scriptId' or 'campaignName' or 'continueScript' or 'lng' params
- in terminal enter command `npm build`
- after that you will have new folder /build with GadgetUI

### Install on Web-Server 



## Nginx
- Setup open to pulbic url on Nginx for '/fs' path pointing to fs server
    ```nginx
    # /fs  public url path resolving
    location /fs {
      proxy_pass http://<FS_URL>:8080;
      proxy_redirect     off;
      proxy_set_header   Host             $host;
      proxy_set_header   X-Real-IP        $remote_addr;
      proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
      proxy_set_header   X-FORWARDED-PROTO https;
      proxy_read_timeout 3000;
      proxy_buffering    on;
      proxy_buffers      16 128k;
      client_max_body_size  100m;
    }
    
    # /SmiddleFileStorage  public url path resolving
    location /SmiddleFileStorage {
      proxy_pass http://<FS_URL>:8080;
      proxy_redirect     off;
      proxy_set_header   Host             $host;
      proxy_set_header   X-Real-IP        $remote_addr;
      proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
      proxy_set_header   X-FORWARDED-PROTO https;
      proxy_read_timeout 3000;
      proxy_buffering    on;
      proxy_buffers      16 128k;
      client_max_body_size  100m;
     }
    
    # /WebConnector  public url path resolving
    location /WebConnector {
      proxy_pass http://<WEB-Connector_URL>:8080;
      #proxy_next_upstream error timeout http_404 non_idempotent;
      proxy_redirect     off;
      proxy_set_header   Host             $host;
      proxy_set_header   X-Real-IP        $remote_addr;
      proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
      proxy_set_header   X-FORWARDED-PROTO https;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_read_timeout 3000;
      proxy_buffering    on;
      proxy_buffers      16 128k;
      client_max_body_size  100m;
     }

    # /Messenger connector public url path resolving
    location /<MessengerConnectorPath> {
      proxy_pass http://<MessengerConnector_URL>:8080;
      proxy_next_upstream error timeout http_404 non_idempotent;
      proxy_redirect     off;
      proxy_set_header   Host             $host;
      proxy_set_header   X-Real-IP        $remote_addr;
      proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
      proxy_set_header   X-FORWARDED-PROTO https;
      proxy_read_timeout 3000;
      proxy_buffering    on;
      proxy_buffers      16 128k;
      client_max_body_size  100m;
     }

    ```

## Tomcat 
- Setup on tomcat change on SmiddleFileStorage public url
    ```properties
    file.base.url.out=https://sm-pd.smiddle.com
    ```


## FE
### Setup Telegram account


### Setup Viber account 

### Setup FB accoutn
