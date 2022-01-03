---        
title: "Add cert to java trust store"        
date: 2022-01-03T23:11:04+02:00        
draft: false                                                        
--- 

Added by next command
${java_home}/bin/keytool -import -alias ${certAliasInStor} -file ${path2cert} -keystore ${path2keystore}
