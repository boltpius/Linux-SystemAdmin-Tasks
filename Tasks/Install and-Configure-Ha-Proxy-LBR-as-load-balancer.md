Summary of the Task:

The task involves installing and configuring HAproxy on the LBR (Load Balancer) server using yum. The goal is to set up HAproxy to balance the load between multiple app servers running Apache on port 8084. Once the configuration is complete, the website can be accessed using the "StaticApp" button on the top bar.

*Steps to Install and Configure HAproxy:*

1. I ssh into the haproxy server.
2. I Installed HAproxy using the command: `sudo yum install haproxy`.
3. I Opened the HAproxy configuration file: `sudo vi /etc/haproxy/haproxy.cfg`.
4. I added the following lines to the configuration file to set up load balancing:

```haproxy
frontend main
    bind *:80
    acl url_static       path_beg       -i /static /images /javascript /stylesheets
    acl url_static       path_end       -i .jpg .gif .png .css .js

    use_backend static          if url_static
    default_backend             app

backend app
    balance     roundrobin
    server  stapp01 172.16.238.10:8084 check
    server  stapp02 172.16.238.11:8084 check
    server  stapp03 172.16.238.12:8084 check

5. I validated the file to see if there are any errors: haproxy -c -f /etc/haproxy/haproxy.cfg
6. I enabled and restarted haproxy using the commands: sudo systemctl enable haproxy && sudo systemctl restart haproxy
7. I confirmed if the site was reachable using curl and checking it in a browser. 
