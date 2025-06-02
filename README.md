# n
:local IPADDRMASK [/ip address get value-name=address [find interface=bridge_private]];
:local IPADDR [:pick $IPADDRMASK 0 [find $IPADDRMASK "/"]];
#log info $IPADDR
:local pingradius1 ([ ping count=10 10.10.40.32 src-address=$IPADDR]);
:local pingradius2 ([ ping count=10 10.10.36.52 src-address=$IPADDR]);
#log info $pingradius1;
#log info $pingradius2;
:local zero 0;
:if ( $pingradius1 = zero && $pingradius2 = zero ) do={
/user set [find where name="divadmin" && disabled=yes] disabled=no
} else={
:if ( $pingradius1 != zero || $pingradius2 != zero ) do={
/user set [find where name="divadmin" && disabled=no] disabled=yes}
}
