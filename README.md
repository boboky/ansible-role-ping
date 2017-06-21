ansible-role-ping
=========

This is a wrapper around ping and win_ping for use in playbooks. It can be used instead of ansible -m ping or ansible -m win_ping. It has the advantage of being able to use the group_vars from the playbook so you can set the winrm settings and also figures out if it windows or linux and runs the right command.


Role Variables
--------------

The group_vars needs to be configured appropriately for windows hosts

e.g

	ansible_user: username
	ansible_password: "{{ username_winrm_password }}"
	ansible_port: 5986
	ansible_connection: winrm
	# The following is necessary for Python 2.7.9+ (or any older Python that has backported SSLContext, eg, Python 2.7.5 on RHEL7) when using default WinRM self-signed certificates:
	ansible_winrm_server_cert_validation: ignore


To set up SSH agent assume you are using SSH keys for authentication and avoid retyping passwords, you can do

	 ssh-agent bash
	 ssh-add ~/.ssh/id_rsa


Host File
----------------

Edit /etc/ansible/hosts and put one or more remote systems in it or create a custome host file in your blaybook

         [windows]
	 10.0.2.150
	 server-a.example.com
	 
	 [linux]
	 10.0.2.152
	 server-b.example.com

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - { role: ansible-role-ping }

If you are calling this role in your galaxy and also using a domain account, you might want to create a group_var/all

    |- group_vars
         |---- all
                |-------main.yml
                |-------vault.yml              #always encrypt {ansile-encrypt /path/to/group_vars/all/vault.yml}

Playbook commands
------------------
Ping the nodes

         ansible all -m ping                                   #for linux nodes
	 ansible all -m win_ping                               #for widows nodes
	 ansible all -m ping -u user --sudo                    # as bruce, sudoing to root
         ansible all -m ping -u user -b --become-user user2    # as bruce, sudoing to <sudoer user>
