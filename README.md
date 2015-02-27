sensu
=====

TODO


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
    sensu_server: my_sensu_server
  roles:
    - role: sensu
      vars:
        sensu_api_host: "{{ sensu_server }}"
        sensu_rabbitmq_host: "{{ sensu_server }}"
        sensu_redis_host: "{{ sensu_server }}"
```


Role variables
--------------

List of variables used by the role:

```
```


License
-------

MIT


Author
------

Robert Readman, Jiri Tyr
