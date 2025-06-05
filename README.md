![photo_2024-10-23_06-46-06](https://github.com/user-attachments/assets/0ab92c61-dd20-4fb5-a985-54b98d7e86d3)
![photo_2024-11-12_21-36-06](https://github.com/user-attachments/assets/70e99c2b-d4b0-41f3-b6c7-cf2427b4baa2)
system script
add dont-require-permissions=no name=radius_check_script owner=admin policy=\
ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source="\r\
\n:local IPADDRMASK [/ip address get value-name=address [find interface=bridge_private]];\r\
\n:local IPADDR [:pick \$IPADDRMASK 0 [find \$IPADDRMASK \"/\"]];\r\
\n#log info \$IPADDR\r\
\n:local pingradius1 ([ ping count=10 10.10.40.32 src-address=\$IPADDR]);\r\
\n:local pingradius2 ([ ping count=10 10.10.36.52 src-address=\$IPADDR]);\r\
\n#log info \$pingradius1;\r\
\n#log info \$pingradius2;\r\
\n:local zero 0;\r\
\n:if ( \$pingradius1 = zero && \$pingradius2 = zero ) do={\r\
\n/user set [find where name=\"divadmin\" && disabled=yes] disabled=no\r\
\n/ip firewall filter set [fin where comment=external_access && disabled=no] disabled=yes\r\
\n} else={\r\
\n:if ( \$pingradius1 != zero || \$pingradius2 != zero ) do={\r\
\n/user set [find where name=\"divadmin\" && disabled=no] disabled=yes}\r\
\n/ip firewall filter set [fin where comment=external_access && disabled=yes] disabled=no}\r\
\n}"
