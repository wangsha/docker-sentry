docker-sentry
============

[![Build Status](https://travis-ci.org/wangsha/docker-sentry.svg?branch=master)](https://travis-ci.org/wangsha/docker-sentry)

Ansible role to manage and run the sentry docker container. 

Requirements
------------

This role has only been tested on Ubuntu 14.04. Since this uses Ansible's
docker module, you will need to ensure that a recent-ish version of `docker-py`
and `docker` are installed.


Examples
--------

Install this module from Ansible Galaxy into the './roles' directory:
```bash
ansible-galaxy install wangsha.docker-sentry -p ./roles
```

If you are installing on a new database, you will need to run upgrade command before starting server.
```bash
$ docker run -it --rm --link sentry-postgres:postgres --link sentry-redis:redis sentry upgrade
```

Use it in a playbook as follows:
Case 1: assuming you already have sentry and postgres running in docker containers:
```yaml
- hosts: 'servers'
  vars:
    docker_sentry_links:
      - "{{ docker_sentry_name }}:sentry"
      - "{{ docker_postgres_name }}:postgres"
    
    docker_sentry_server_expose:
      - 9000
    docker_sentry_server_ports:
      - 9200:9000
      
  roles:
    - role: 'wangsha.docker-sentry'
      become: true
```

Case 2: link sentry and postgres via environment variables.
```yaml
- hosts: 'servers'
  vars:
    
    docker_sentry_env:
      SENTRY_SECRET_KEY: "changethiskey"
      SENTRY_REDIS_HOST: "sentry"
      SENTRY_REDIS_PORT: 6379
      SENTRY_REDIS_DB: 0
      SENTRY_POSTGRES_HOST: "postgres"
      SENTRY_POSTGRES_PORT: ""
      SENTRY_DB_NAME: "postgres"
      SENTRY_DB_USER: "postgres"
      SENTRY_DB_PASSWORD: ""
    
    docker_sentry_server_expose:
      - 9000
    docker_sentry_server_ports:
      - 9200:9000

  roles:
    - role: 'wangsha.docker-sentry'
      become: true
```
Have a look at the [defaults/main.yml](defaults/main.yml) for role variables
that can be overridden! 

Reference Roles:
* [angstwad.docker_ubuntu](https://github.com/angstwad/docker.ubuntu)
* [wangsha.docker-postgres](https://github.com/wangsha/docker-postgres)
* [wangsha.docker-redis](https://github.com/wangsha/docker-redis)

License
-------

[MIT](LICENSE.txt)

Author Information
------------------

- wangsha
