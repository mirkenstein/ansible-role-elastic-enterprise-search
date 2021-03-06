Role Name
=========

A brief description of the role goes here.

Requirements
------------
### Java 11 or lower
```shell
ansible-galaxy install lean_delivery.java
```


```shell script
cd roles
ln -s  /path/to/ansible-role-elastic-ent-search/ ent-search

ansible-playbook -i  inventory-ent-search.yml playbook-ent-search.yml 
```


Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.
Generate your own CA file for HTTPS connectivity to the elasticsearch cluster.
```shell script
openssl s_client -showcerts \
-connect es-node.example.com:443 \
</dev/null 2>/dev/null|openssl x509 \
-outform PEM >server-ca.pem
```
Place the ca file in `files/certs/`

Role Variables
--------------
See defaults/main.yml file.

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

`secret_management.encryption_keys` setting is controlled cia the `enc_key`. If not specified a random string is generated.

Dependencies
------------
https://www.elastic.co/guide/en/kibana/7.9/settings.html
https://www.elastic.co/guide/en/kibana/7.9/elasticsearch-mutual-tls.html
https://swiftype.com/documentation/ent-search/self-managed/configuration
A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```yaml
- name: Install and Configure Elastic Enterprise-search
  hosts: server
  roles:
    - role: lean_delivery.java
      java_major_version: 11
      java_tarball_install: true
      transport: web
      transport_web: https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz
    - role: ent-search
  vars:
    es_host: "{{hostvars[inventory_hostname].es_host}}"
    es_ent_search_user: "{{hostvars[inventory_hostname].es_ent_search_user}}"
    es_ent_search_pass: "{{hostvars[inventory_hostname].es_ent_search_pass}}"
    es_version: 7.15.1
    es_use_repository: false
    es_ssl_enabled: true
    es_ssl_ca: "files/certs/{{hostvars[inventory_hostname].es_ssl_ca_filename}}"
    #es_ssl_certificate: "files/certs/{{hostvars[inventory_hostname].es_ssl_certificate_filename}}"
    #es_ssl_key:  "files/certs/{{hostvars[inventory_hostname].es_ssl_key_filename}}"
    #es_ssl_key_password: "{{hostvars[inventory_hostname].es_ssl_key_pass}}"
    es_ssl_verify: "true"
    #Host and URL
    ent_search_listen_host: "{{ ansible_default_ipv4.address }}"
    #    ent_search_listen_host: "{{hostvars[inventory_hostname].listen_host}}"
    ent_search_listen_port: "{{hostvars[inventory_hostname].listen_port}}"
    ent_search_external_url: "{{hostvars[inventory_hostname].external_url}}"
    #SSL
    ent_search_ssl_enabled: true
    ent_search_ssl_keystore: "files/certs/{{hostvars[inventory_hostname].keystore_filename}}"
    ent_search_ssl_keystore_password: "{{hostvars[inventory_hostname].keystore_pass}}"
    ent_search_ssl_keystore_key_password:  "{{hostvars[inventory_hostname].keystore_key_pass}}"


```


Inventory file
----------------
```shell
[server]
ssh-server-alias
[server:vars]
#Elasticsearch connection settings
es_host=es1-node.example.com
es_ent_search_user=elastic
es_ent_search_pass=changeme
cert_pass=myCertPass
es_ssl_ca_filename=server-ca.pem
#App search settings
listen_host=10.0.0.11
listen_port=3002
external_url=https://example.com:3002
keystore_filename=example.com.pfx
keystore_pass=p12KeystorePassHere
keystore_key_pass=p12KeystorePassHere

```
    
License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
