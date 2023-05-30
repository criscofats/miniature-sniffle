# AAP setup from scratch WIP:
- Copy /etc/hosts to all hosts
- Copy ssh keys between all hosts
- Copy/create insights tags.yml file for hosts `/etc/insights-client/tags.yml`
### Example:
```
---
group:
  - Hyper-V
  - Virtual Machine
  - RHEL9
  - Lab
  - AAP
  - Ansible Automation Platform/Private Automation Hub
location:
  - Home
description:
  - RHEL9 Hyper-V virtual machine running Ansible Automation Platform
```

- Run sudo ./setup.sh on controller
- Set up new root certs between controller and automation hub or copy generated ones
    - `sudo scp ansible-automation-platform-managed-ca-cert.crt controller:/etc/pki/ca-trust/source/anchors/pah-ca-cert.crt`
    - `openssl req -newkey rsa:4096 -x509 -sha256 -days 3650 -nodes -out controllercert.crt -keyout controllerkey.key`
    - `openssl req -newkey rsa:4096 -x509 -sha256 -days 3650 -nodes -out automationhubcert.crt -keyout automationhubkey.key`
    - Run `sudo update-ca-trust` on all hosts
    - If you want to be able to use the shortnames you need to use a '*'
```
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:US
State or Province Name (full name) []:State
Locality Name (eg, city) [Default City]:City
Organization Name (eg, company) [Default Company Ltd]:Org
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []:hostname*
Email Address []:
[cmcaskil@automationhub certs]$ sudo update-ca-trust
```
- Add miniature-sniffle repo to controller host `git clone git@github.com:criscofats/miniature-sniffle.git`
- Restore from miniature-sniffle repo `playbooks/controller_config.yml`
- Add credentials to Default organization for automation hub functionality
- Export commands `awx --conf.host https://magpie --conf.username admin --conf.password 'password' --conf.insecure export > allassets` `awx --conf.host https://controller --conf.username admin --conf.password 'password' --conf.insecure import < all assets`