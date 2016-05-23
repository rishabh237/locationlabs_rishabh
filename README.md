# locationlabs_rishabh
Location labs project:

1. This project configures apache2 webserver with ansible.
2. Locations:
   loclab/apache_ansible directory : This contains the ansible YAML file (apache.yml), local ansible.conf, hosts and apache virtualhost.conf file.
   loclab/sshconfig directory : This contains the SSH configuration files, RSA key. (It's just a backup)
3. Background:
   The project is created in Oracle Virtualbox --> Ubuntu 14.04 linux box.
  Server 1: Richmond (Primary)- Anisble resides here.
  Server 2: Sunset (Secondary)- Apache to be configured on this node via ansible.

  Steps performed to configure the servers:
  
                1) Configure hosts:
                
                added the entry(Richmond & Sunset) to /etc/hosts 
                
                2) Create a non-root user (Richmond & Sunset):
                # useradd -m /home/depansi depansi
                # passwd depansi
                # adduser depansi sudo
                
                3) Change the ssh port to 50000 (Richmond & Sunset):
                change the port in /etc/ssh/sshd_config to 50000
                Restart the ssh daemon : # service ssh restart
                
                4) Generated RSA keys(Richmond & Sunset):
                # ssh-keygen -t RSA
                
                5) Copy the keys to other node:
                richmond# ssh-copy-id depansi@sunset -p 50000
                
                6) Install & Configure NTP(Richmond & Sunset):
                # apt-get update
                # apt-get install ntp
                 
                # dpkg-reconfigure tzdata (Already was America/Los Angeles, so no change)
                 
                7) Enable firewall & allow ports(Richmond & Sunset):
                
                # ufw enable
                # ufw allow http
                # ufw allow ftp
                # ufw allow https
                # ufw allow ntp
                # ufw allow 50000
                # ufw allow ssh
                
                8) Install ansible (only Richmond):
                # apt-get install software-properties-common
                # apt-add-repository ppa:ansible/ansible
                # apt-get update
                # apt-get install ansible
                
                9) Changed the host and port in /etc/ansible/hosts:
                Added sunset:50000
                (later on created local "hosts" and "ansible.cfg" file in /home/ansible/loclab/apache_ansible/.)
                
                10) Tested a simple ping module, got pong!
                
  4. Ansible configuration:
              Following modules have been added in playbook:
                  1. ping
                  2. update apt
                  3. install apache2
                  4. enabling mod_rewrite
                  5. change port of apache2 to 51000
                  6. Added handlers to bounce apache2
                  7. change apache2 VirtualHost to point to 51000
                  8. allow firewall for port 51000
                  9. create a new virtualhost file using a template, added vars to YML.
                  10. enable the virtual hosts

  5. Known Issues:
                  1. Indentation and syntax errors 
                  
                
                
                
                
  
  
  
