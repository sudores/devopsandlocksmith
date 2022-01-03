---        
title: "Enable ssl debug tomcat"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 
Add option to java java startup options "-Djaavx.net.debug=all"

Could be done with adding it to next files
${catalina.home}/bin/startup.sh ${catalina.home}/bin/setenv.sh

Or to systemctl unit file
JAVA_OPTS="$JAVA_OPTS -Djavax.net.debug=all"
