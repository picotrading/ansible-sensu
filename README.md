sensu
=====

Ansible role to install and configure Sensu and Uchiwa dashboard.

The configuraton of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Example
-------

```
---

# Server installation
- hosts: my_sensu_server
  roles:
    - rabbitmq
    - redis
    - uchiwa
    - role: sensu
      sensu_install_server: true
      sensu_install_api: true

# Client installation
- hosts: my_client1
  vars:
    server__sensu: my_sensu_server
  roles:
    - role: sensu
      vars:
        sensu_api_host: "{{ server__sensu }}"
        sensu_rabbitmq_host: "{{ server__sensu }}"
        sensu_redis_host: "{{ server__sensu }}"
```


Role variables
--------------

List of variables used by the role:

```
# YUM repo URL (change "el/6" to "$releasever" once there is repo for "el/7")
sensu_yum_repo_url: http://repos.sensuapp.org/yum/el/6/$basearch/

# Package to be installed (you can force a specific version here)
sensu_pkg: sensu

# This allows to change the name of handlers
# (handy if the role is encapsualted and called multiple times)
sensu_from: "(default)"

# Variables for installation of client/server
sensu_install_server: false
sensu_install_api: false
sensu_install_client: true

# Default sensu-server config variables
sensu_rabbitmq_host: localhost
sensu_rabbitmq_port: 5671
sensu_rabbitmq_user: sensu
sensu_rabbitmq_password: sensu
sensu_rabbitmq_vhost: /sensu
sensu_redis_host: localhost
sensu_redis_port: 6379
sensu_handlers_default_type: pipe
sensu_handlers_default_command: " true"

# Default sensu-api config variables
sensu_api_host: localhost
sensu_api_port: 4567

# Default sensu-server and sensu-api config
sensu_config:
  rabbitmq:
    host: "{{ sensu_rabbitmq_host }}"
    port: "{{ sensu_rabbitmq_port }}"
    user: "{{ sensu_rabbitmq_user }}"
    password: "{{ sensu_rabbitmq_password }}"
    vhost: "{{ sensu_rabbitmq_vhost }}"
  redis:
    host: "{{ sensu_redis_host }}"
    port: "{{ sensu_redis_port }}"
  api:
    host: "{{ sensu_api_host }}"
    port: "{{ sensu_api_port }}"
  handlers:
    default:
      type: "{{ sensu_handlers_default_type }}"
      command: "{{ sensu_handlers_default_command }}"

# Default sensu-client config variables
sensu_client_name: "{{ ansible_hostname }}"
sensu_client_address: "{{ ansible_default_ipv4.address }}"
sensu_client_subscriptions:
  - test

# Default client config
sensu_client_config:
  client:
    name: "{{ sensu_client_name }}"
    address: "{{ sensu_client_address }}"
    subscriptions: "{{ sensu_client_subscriptions }}"

# Default RabbitMQ config for Sensu client
sensu_client_rabbitmq_config:
  rabbitmq:
    host: "{{ sensu_rabbitmq_host }}"
    port: "{{ sensu_rabbitmq_port }}"
    user: "{{ sensu_rabbitmq_user }}"
    password: "{{ sensu_rabbitmq_password }}"
    vhost: "{{ sensu_rabbitmq_vhost }}"
```


Dependencies
------------

* [`yumrepo`](https://github.com/picotrading/ansible-yumrepo) role
* [Config Encoder Macros](https://github.com/picotrading/config-encoder-macros)


License
-------

MIT


Author
------

Robert Readman, Jiri Tyr
