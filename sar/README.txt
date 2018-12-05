Sar Template: Installation and usage guide

Follow these few steps:

1. Install the package which contains sar utilities (such as sa1,sadf, etc).

    It's possibly named like "sysstat".

    For RPM distroes:
    # yum install sysstat

    For DEB distroes:
    # apt-get install sysstat
    
2. Configure sar collector
    To get full awareness about the system, you must change settings.
    The path of the configuration files depends on the distro.
    
    For RPM distroes:
    # /etc/sysconfig/sysstat
    
    For DEB distroes:
    # /etc/sysstat/sysstat
    
    Please change the collector option to the folllowing:
    # SADC_OPTIONS="-S XALL"
    It gives you complete information about everything happened with your server.
    Of course, some metrics I disabled and some I found useless because I developed this template for VMs. This means that the repo is opened for issues and discussions.
    
    Using DEB distroes, don't forget to check whether the sa1 collector is enabled.
    /etc/default/sysstat
    # ENABLED="true"
    
3. Check all to be working
    As you know, sa1 collector is run periodically by cron daemon.
    So check if cron is working (according to your initialization system):
    # service cron status
    # /etc/init.d/crond status
    # systemctl status crond
    
    Check that the sysstat file (usually delivered by distro package) is present in the cron configs:
    # ls /etc/cron.d/sysstat
    
    After several minutes (10 min by default) check that sar got and can show values about system:
    # sar -A 
    # sadf -- -A
    
4. Update zabbix agent configuration
    Copy zabbix keys file for sar:
    # cp sar.conf /etc/zabbix/zabbix_agentd.d/
    Restart zabbix agent daemon:
    # service zabbix-agent restart
    # /etc/init.d/zabbix-agent restart
    # systemctl restart zabbix-agent

5. Problems
    The issue happening much often is no extra metrics data after sar collector reconfiguration (step 2). This is due to data files which formerly formatted without extra fields. The easiest way to solve this case is to delete old collected data.
    
    For RPM distroes:
    # rm -f /var/log/sa/*
    
    For DEB distroes:
    # rm -f /var/log/sysstat/*
    
    Don't worry about previous data. Zabbix agent gets only the last value.
    
    Also be ready to watch errors in zabbix server log about keys unavailability. The template keys rely on specific sar data fields' names. Thus, on the testing distro branches it can appear from time to time after updates.
    
6. Tips
    You may want to reduce the time interval of data obtainment.
    Change crontab definition in the file: 
    # /etc/cron.d/sysstat
    
    Here is example given for RHEL/Centos:
    <<<<    */10 * * * * root /usr/lib64/sa/sa1 -S XALL 1 1
    >>>>    */5 * * * * root /usr/lib64/sa/sa1 -S XALL 1 1
    and
    <<<<    53 23 * * * root /usr/lib64/sa/sa2 -A
    >>>>    58 23 * * * root /usr/lib64/sa/sa2 -A
    
    Then you should change update interval for every item in the template.

Now you are ready to import sar_zabbix_template.xml into your Zabbix.

Have a nice day.

Sergey Vokhmyanin
