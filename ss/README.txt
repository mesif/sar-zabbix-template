SS Template: Installation and usage guide

Follow these few steps:

1. Update zabbix agent configuration
    Copy zabbix keys file for sar:
    # cp ss.conf /etc/zabbix/zabbix_agentd.d/
    Restart zabbix agent daemon:
    # service zabbix-agent restart
    # /etc/init.d/zabbix-agent restart
    # systemctl restart zabbix-agent

2. Problems
    This template may generate noticable load on the huge instances like Proxy, CDN, NAT, VoIP.
    To avoid overloads and to mitigate influence of zabbix agent checks:
    - Allocate at least 1 core thread for template checks.
    - Increase zabbix agent StartAgents (10 should be enough)
    - Increase zabbix agent Timeout (10 sec is pretty)
    
    You can also disable IP discovery for "IP-bounced" installations.

Have a nice day.

Sergey Vokhmyanin
