It is Ansible playbooks. They were tested with CentOS 7.
Playbooks are available via https://git.tmaws.io/ipi/ansible-kickstart

This Repo included list of ansible playbooks:
- Postinstall_servers_configurations
- Cloudstack_compute_install_ansible
- Baremetal_servers_installation

The conf task number is https://contegixapp1.livenation.com/jira/browse/IPI-454 (Child of IPI-439)

How to use:
cd postinstall_servers_configurations/
ansible-playbook main.yml -i inventory	(fill list of postinstal roles)
ansible-playbook main.yml -i inventory -t rsyslog
ansible-playbook main.yml -i inventory -t updates,rsyslog,rpm

1.) Postinstall_servers_configurations roles:

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

2.) Cloudstack_compute_install_ansible roles:

 - comming soon
 - comming soon
 - comming soon

3.) Baremetal_servers_installation: (do we need it?)

 - comming soon
 - comming soon
 - comming soon

Please do a reverse lookup on the assigned IP address and set the hostname to that
