Ansible role: GitBucket
=========

[GitBucket](https://github.com/gitbucket/gitbucket) is a Git platform powered by Scala with easy installation, high extensibility & github API compatibility.

Requirements
------------

None.

Role Variables
--------------

    gitbucket_version: 4.12.1

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: shomatan.gitbucket }

### Example nginx.conf

    server {

        listen 80;
        server_name localhost;

        access_log /var/log/nginx/gitbucket-access.log;
        error_log  /var/log/nginx/gitbucket-error.log error;

        proxy_set_header Host               $host;
        proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host   $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Real-IP          $remote_addr;

        location / {
            proxy_pass              http://127.0.0.1:8080/gitbucket/;
            proxy_connect_timeout   150;
            proxy_send_timeout      100;
            proxy_read_timeout      100;
            proxy_buffers           4 32k;
            proxy_cookie_path       /gitbucket/ /;
            client_max_body_size    500m; # Big number is we can post big commits.
            client_body_buffer_size 128k;
        }

        location /assets/ {
            proxy_pass http://127.0.0.1:8080/gitbucket/assets/;
        }
    }

License
-------

BSD

Author Information
------------------

Shoma Nishitateno