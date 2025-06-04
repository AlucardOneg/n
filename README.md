# Активируем ТОЛЬКО наши специальные правила (без влияния на другие)
    /ip firewall filter enable [find where comment~"emergency-mgmt-access"];
} else={
    # Если хотя бы один RADIUS-сервер доступен:
    /user set [find where name="divadmin" && disabled=no] disabled=yes;
    :log info "RADIUS servers available - disabling divadmin and blocking external management";

    # Деактивируем ТОЛЬКО наши специальные правила
    /ip firewall filter disable [find where comment~"emergency-mgmt-access"];
}



/ip firewall filter add chain=input protocol=tcp dst-port=8291 action=accept comment="emergency-mgmt-access-winbox" disabled=yes;
/ip firewall filter add chain=input protocol=tcp dst-port=22 action=accept comment="emergency-mgmt-access-ssh" disabled=yes;
