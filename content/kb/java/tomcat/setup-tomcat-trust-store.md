---        
title: "Add key/cert to tomcat trust store"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 
Add options below to setenv or startup

```bash
"-Djavax.net.ssl.trustStore=/tomcat/server.truststore -Djavax.net.ssl.trustStorePassword"
```

e.g

```bash 
JAVA_OPTS="$JAVA_OPTS -Djavax.net.ssl.trustStore=${path2it} -Djavax.net.ssl.trustStorePassword=changeit"
```
