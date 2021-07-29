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

  For e.g.
  
  ![image](https://github.com/harshkhare3/nginx_reverse_proxy/blob/master/subdomain_example.png?raw=true)

  4. Then, go to the SSL section and add a pre-existing wildcard SSL cert to this proxy host. Select 'Force SSL' and 'HTTP/2 Support' options as well.

  5. Now, go to your DNS settings and add a **CNAME** record for your website.

And that's it. You are done!

### Option 2 (location)

To add a website in this fashion 'example.com/site1', perform the following steps :-

   1. Navigate to the Nginx Proxy Manager's web UI by going to the servers IP address (port 81).

   2. Go to 'Proxy Host', and click on 'Add Proxy Host'.

   3. Fill in the domain name (e.g.example.com) and Forward Hostname/IP. The forwarded Hostname/IP will be the same as your domain name (e.g. example.com) Use http mode and forward it to port 80.

   4. Navigate to Custom Locations and add a new location (e.g. /site1). Write the name of the docker container of the website that is to be accessed (e.g. site1_app_1) in Forward Hostname/IP field. Use http mode and forward it to port 80.

   5. Then, go to the SSL section and add a pre-existing wildcard SSL cert to this proxy host. Select 'Force SSL' and 'HTTP/2 Support' options as well.

And that's it. You are done!

## Contact

Harsh Khare - harshkhare3@gmail.com

Project Link: [https://github.com/your_username/repo_name](https://github.com/your_username/repo_name)
