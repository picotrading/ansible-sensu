Sensu-server
=======

Role to install Sensu-server and Sensu-client.

By default Server is not installed.
By default Client is installed, server and api services remain present but disabled.

Defaulting to not use SSL with rabbitmq, define rabbitmq client cert and key file to enable it.

Dashboard configuration is not present in the client.json file since sensuapp no longer publishes their own dashboard.

The server configuration defaults to localhost for all services. For a client configuration you can override the api, rabbitmq and redis host vars to point to the server name.

Example
-------

```
---

- hosts: myhost
  roles:
    - role: sensu
      vars:
        install_sensu_server: true
        redis_host: redis-server
        rabbitmq_host: rabbitmq-server

- hosts: client_host
  roles:
    - role: sensu
      vars:
        client_name: "this-host"
        sensu_api_host: sensuvm
        sensu_rabbitmq_host: sensuvm
        sensu_redis_host: sensuvm
```

Role variables
--------------

List of variables used by the role:

```
# Repo
sensu_repo: http://repos.sensuapp.org/yum/el/$releasever/$basearch/

# Variables for installation of client/server
install_sensu_server: false
install_sensu_client: true

# Cert and key file
client_cert: client_cert.pem
client_key: client_key.pem

# Common defaults
sensu_config_dir: /etc/sensu
sensu_ssl_dir: /etc/sensu/ssl

# Sensu rabbitmq config
sensu_rabbitmq_cert_file: "{{ sensu_ssl_dir }}/{{ client_cert }}"
sensu_rabbitmq_key_file: "{{ sensu_ssl_dir }}/{{ client_key }}"

# Client defaults
sensu_client_name: {{ ansible_hostname }}
sensu_client_address: {{ ansible_eth0.ipv4.address }}
sensu_client_subscriptions: "test"

# Server defaults for config.json
api_host: localhost
api_port: 4567
dashboard_host: localhost
dashboard_port: 8080
dashboard_user: admin
dashboard_password: secret
handlers_type: pipe
handlers_command: true
sensu_rabbitmq_host: sensuvm
sensu_rabbitmq_port: 5671
sensu_rabbitmq_vhost: /sensu
sensu_rabbitmq_user: sensu
sensu_rabbitmq_password: sensu
redis_host: localhost
redis_port: 6379
```

License
-------

MIT


Author
------

Robert Readman
