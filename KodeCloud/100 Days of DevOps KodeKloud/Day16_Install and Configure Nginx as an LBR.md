Task:

Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. 
Therefore, the team has observed a degradation in website performance. 
Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. 
They started the migration last month and it is almost done, as only the LBR server configuration is pending. 

Configure LBR server as per the information given below: 
a. Install nginx on LBR (load balancer) server. 
b. Configure load-balancing with the an http context making use of all App Servers. 
   Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.

Requirements:
1. ssh to LBR server
2. check what is the port on which Apache is listening for all the app servers: by doing ssh on all the servers
   sudo ss -tulnp | grep httpd
   
   if ss doesn't work;
   sudo netstat -tulnp | grep httpd
  

3. Install nginx on the LBR server.
   
sudo yum install -y nginx   # CentOS/RHEL
sudo systemctl enable nginx
sudo systemctl start nginx


4. Add and modify under /etc/nginx/nginx.conf
   
a. Inside http { ... }, add this just above the existing server { ... }:
upstream app_servers {
        server 172.16.238.10:80;
        server 172.16.238.11:80;
        server 172.16.238.12:80;
    }


 b. Then change your existing server { ... } block from this:

        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;


to this (replace the root and include with proxy_pass):

        location / {
            proxy_pass http://app_servers;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
Define an upstream block with all App Servers (stapp01, stapp02, stapp03).

5. After making changes:
sudo nginx -t        # test config
sudo systemctl restart nginx

6. to verify from jumphost
   curl -I http://<LBR-IP>/

