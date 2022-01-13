Utilizing ansible playbook https://github.com/dev-sec/ansible-collection-hardening, https://github.com/geerlingguy/ansible-role-security, and https://github.com/geerlingguy/ansible-role-firewall

Run './requirements' to install the required ansible scripts

Modify vars files as needed; 'fw.yml', and 'sec.yml'. 

Export varibles if you need to use password auth for ssh.
```
export username=<user>
export password=<password>
```

Then, run;
'ansible-playbook hardening.yml -i hosts -c paramiko --user=$username --ask-pass --extra-vars "ansible_sudo_pass=$password"'
or
'ansible-playbook hardening.yml -i hosts -c paramiko'






##Vagrant setup

We can start the server with:
`vagrant up`
Now we have to add it to Ansible's inventory, but we need to know the ssh key Vagrant is using when we connect using vagrant ssh:
```
$ vagrant ssh-config
Host tau
  HostName 127.0.0.1
  User vagrant
  Port 2200
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/javiervidal/test/.vagrant/machines/tau/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```
Interesting, we can use `/Users/javiervidal/test/.vagrant/machines/tau/virtualbox/private_key` in the inventory. We need to add a line like this:
`tau ansible_host=192.168.27.2 ansible_port=22 ansible_ssh_user=vagrant ansible_ssh_private_key_file=/Users/javiervidal/test/.vagrant/machines/tau/virtualbox/private_key ansible_python_interpreter=/usr/bin/python3`
And finally we can test that Ansible can connect to tau:
```
$ ansible tau -m ping   
tau | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

