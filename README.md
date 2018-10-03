These are Ansible playbooks. They were tested with CentOS 7.
Playbooks are available via https://git.tmaws.io/ipi/ansible-kickstart

This Repo included 2 ansible playbooks:
- Postinstall_servers_configurations
- Cloudstack

1.) Postinstall_servers_configurations roles:
The conf task number is https://contegixapp1.livenation.com/jira/browse/IPI-454 (Child of IPI-439)

How to use:
cd postinstall_servers_configurations/
ansible-playbook main.yml -i inventory	(fill list of postinstal roles)
ansible-playbook main.yml -i inventory -t rsyslog
ansible-playbook main.yml -i inventory -t updates,rsyslog,rpm

- updates:	*	*	*	Install all latest updates on a system, Adding the internal ticketmaster repositaries, Adding the proxy parameters in yum.conf.
- rpm:		*	*	*	Install additional RPM packages on a system.
- auth:		*	*	*	Change 'PasswordAuthentication' and 'PermitRootLogin' parameters in /etc/ssh/sshd_config, restart ssh with new parameters.
- firewalld:	*	*	*	Templates "how to use firewall", disable firewalld, stop firewalld.
- ntp:		*	*	*	Install and configure chrony service as NTP service.	
- resolv-conf:	*	*	*	Adding nameservers to the resolf.conf file.
- selinux:	*       *       *       Disable selinux and set 'permissive' state of that.
- static-ip:	*       *       *       Changing all network interfaces from DHCP to static configurations. Stop and disable NetworkManager.
- users:	*       *       *       Adding main list of users ('nameofuser' temporary user only now, as template)
- hostname:	*       *       *       Keep temporary old hostname, adding hostname and fqdn of server in /etc/hosts.
- common:	*       *       *       Install additional RPMs for backup configuratopn, Configuration of UTC timezone
- ipv6:		*       *       *       Disable IPv6 with sysctl
- sshd:		*       *       *       Install openssh, ensure permissions, configure banner for SSH "Welcome". Change 'PasswordAuthentication' and 'PermitRootLogin' parameters.
- iptables:	*       *       *       (DISABLED) Configure iptables roles for 22,3022,9100,16514 ports
- rsyslog:	*       *       *       Change rsyslog.conf and export all logs to log-server, restart rsyslog service.
- users:	*       *       *       Add users, keys, catalogs permissions .... for rwebb,bdixie,mrowell,rcastrogiovanni,ipi
- akfgen:	*       *       *       Add Ticketmaster Repo,Install and configure AKFGEN service
- ciscat:	*       *       *       CISCAT-4.1 Configure System Accounting (service, cron, hosts.allow, configfile)

2.) Cloudstack roles:

This playbook can be used for eazy cloudstack controllers installation.
It can be used for 2-controller configurations. Roles were tested with CentOS 7.
Strongly recomended to run "Postinstall_servers_configurations roles" before "Cloudstack roles" to prepare servers for internal infrastrusture
The main part of roles based on https://contegixapp1.livenation.com/confluence/pages/viewpage.action?pageId=150345377
The JIRA task is https://contegixapp1.livenation.com/jira/browse/IPI-389

How to use:

### This way for cleaning server (remove rpms,directories,configs,db ...)
cd cloudstack/
ansible-playbook remove-all-installation.yml -i inventory

### This way to install cloudstack clusters on new servers
cd cloudstack/
ansible-playbook main.yml -i inventory

Some notes:

* - NFS mount for /var/lib/mysql is ready, it is disabled now. 2 controllers cant work with the same /var/lib/mysql on the same time. Mariadb cluster are working as one synked cluster.


2. Set the snapdir-access property to false on that FlexVol until Mariadb initializes the datadir (or it will fail thinking it's not empty)
 - is that nfs share /var/lib/mysql ?
6. roleid ?
7. check "Disable SNI" part
