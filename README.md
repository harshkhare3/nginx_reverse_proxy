# About the Project

This is a nginx reverse proxy.
A Nginx HTTPS reverse proxy is an intermediary proxy service which takes a client request, passes it on to one or more servers, and subsequently delivers the server's response back to the client.

This project uses [Nginx Proxy Manager](https://github.com/jc21/nginx-proxy-manager) to add websites to the reverse proxy.

## Getting Started

Follow the steps on ths [link](https://nginxproxymanager.com/guide/#quick-setup) to set up Nginx Proxy Manager on your local machine.

## Usage

This section will show you how to add a client server for the reverse proxy.

To create a server in docker, create a docker container for your website and add the following line of code to `docker-compose.yml` file.

```docker
version: "3.5"
services:
  app:
    ...
    networks:
      - proxy
      
networks: 
  proxy:
    external: 
      name: nginxproxymanager_default

```

Note - The `networks` derictive creates a network called `proxy` between this app and the reverse proxy only that is used to accept requests from the reverse proxy. Please ensure atleast one port is exposed.

See [site1](/site1/docker-compose.yml) for an example.

The reverse proxy can be configured in two ways to connect to a client server.

### Option 1 (subdomain)

Perform the following steps to setup a server to have its own subdomain (e.g. site1.example.com) :

   1. Navigate to the Nginx Proxy Manager's web UI by going to the servers IP address (port 81).

   2. Go to **Proxy Host**, and click on **Add Proxy Host**.

   3. Fill in the domain name (e.g. site1.example.com) and Forward Hostname/IP name. Use http mode and forward it to port that you have exposed.
   In Forward Hostname/IP section, write the name of the docker container of your website.

NOTE - To find the name of your website's docker container, run '`docker ps`' command on your local machine.

```console
CONTAINER ID   IMAGE                             COMMAND                  CREATED       STATUS                 PORTS                                            NAMES
86cc3968a57f   nginx                             "/docker-entrypoint.…"   6 days ago    Up 6 days              0.0.0.0:49189->80/tcp                            site3_app_1
dcb38bf0ded1   jc21/nginx-proxy-manager:latest   "/init"                  11 days ago   Up 11 days (healthy)   0.0.0.0:80-81->80-81/tcp, 0.0.0.0:443->443/tcp   nginxproxymanager_app_1
b2e868db6e52   jc21/mariadb-aria:latest          "/scripts/run.sh"        11 days ago   Up 11 days             3306/tcp                                         nginxproxymanager_db_1
c526d3879085   nginx                             "/docker-entrypoint.…"   13 days ago   Up 13 days             0.0.0.0:49188->80/tcp                            site2_app_1
a6d598e58edd   nginx                             "/docker-entrypoint.…"   2 weeks ago   Up 2 weeks             0.0.0.0:49187->80/tcp                            site1_app_1
```

In the example above, the container name is under the `NAMES` column called `site1_app_1`.

  For e.g.
  
  ![image](images/subdomain_example.png)

  4. Then, go to the SSL section and add a pre-existing wildcard SSL cert to this proxy host. Select 'Force SSL' and 'HTTP/2 Support' options as well.

  5. Now, go to your DNS settings and add a **CNAME** record for your website.

And that's it. You are done!

### Option 2 (location)

To add a website in this fashion 'example.com/site1', perform the following steps :-

   1. Navigate to the Nginx Proxy Manager's web UI by going to the servers IP address (port 81).

   2. Go to 'Proxy Host', and click on 'Add Proxy Host'.

   3. Fill in the domain name (e.g.example.com) and Forward Hostname/IP. The forwarded Hostname/IP will be the same as your domain name (e.g. example.com). Use http mode and forward it to port 80.
  
  ![location_example](images/location_example.png)

   4. Navigate to Custom Locations and add a new location (e.g. /site1). Write the name of the docker container of the website that is to be accessed (e.g. site3_app_1) in Forward Hostname/IP field. Use http mode and forward it to port 80.

   ![location_example2](images/location_example2.png)
   5. Then, go to the SSL section and add a pre-existing wildcard SSL cert to this proxy host. Select 'Force SSL' and 'HTTP/2 Support' options as well.

And that's it. You are done!

## Contact

Harsh Khare - harshkhare3@gmail.com

Project Link: [https://github.com/harshkhare3/nginx_reverse_proxy](https://github.com/harshkhare3/nginx_reverse_proxy)
