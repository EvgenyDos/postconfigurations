# cloudstack_installer

IT ISNT readmi file - It is temporary questions file for me (edos)

Correct READMI file will be created later

ansible-playbook -i inventory.cfg -u root cloudstack-controller.yml

1. On the first controller, make sure you mount the clouddb NFS volume on /var/lib/mysql. Do not mount on the secondary controller.
 - nfs path ?
 - specific params ?
2. Set the snapdir-access property to false on that FlexVol until Mariadb initializes the datadir (or it will fail thinking it's not empty)
 - is that nfs share /var/lib/mysql ?
3. add\change vars sql
4. move sql cred to one separated file?
5. change templates/vip-001.conf to use extermal params (jinja like in postconfigurations)
6. roleid ?
7. check "Disable SNI" part
8. update mysql root password - need?


recheck this parts:
============
    Set global setting consoleproxy.url.domain to *.svm.${CSDOM}
    Restart cloudstack management service on each controller
    In DNS make sure each public IP assigned to the zone resolves as IP-IP-IP-IP.svm.${CSDOM}          
    Upload SSL cert under Infrastructure
        Under CA, use fullchain.cer content
        No intermediate
        Use cert and key generated above
        Set domain to *.svm.${CSDOM}

System VMs will reboot with the new certs.  

Note: If they fail to reboot or take HTTPS connecitons, detroy them so they get recreated.
===========
Generate a keystore to enable LDAP connections over TLS

    For tmaws.io certs:  extract the CA with "openssl s_client -connect  ldaps.pci-tmaws.io:636 -showcerts"
    keytool -import -trustcacerts -alias root -file root.crt -keystore KeyStore.jks
    Copy keystore to all controllers under /etc/cloudstack/management/
    Edit global settings
        ldap.truststore   set to /etc/cloudstack/management/techops
        ldap.truststore.password
    Restart cloudstack-management service on all controllers
    Add LDAP configuration on port 636.  Under Global Settings -  LDAP configuration view - 
=========
