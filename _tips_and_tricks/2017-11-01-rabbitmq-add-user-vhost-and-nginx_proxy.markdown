---
title:  "RabbitMQ - add user, vhost and nginx proxy"
date:   2017-11-07 13:51:00
description: How to fast managing RabbitMQ 
---

Add user and vhost:
{% highlight bash %}
rabbitmqctl add_vhost /test
rabbitmqctl add_user test test
rabbitmqctl set_permissions -p /test test "^test-.*" ".*" ".*"
{% endhighlight %}

Nginx configuration for RabbitMQ Managment:
{% highlight bash %}
server {
    listen 80;
    server_name example.com;

    auth_basic "Restricted";
    auth_basic_user_file /etc/nginx/conf.d/.htpasswd_rabbit;

    location / {
        proxy_pass http://localhost:15672;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
{% endhighlight %}