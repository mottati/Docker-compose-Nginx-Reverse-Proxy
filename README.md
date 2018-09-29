Docker-compose-Nginx-Reverse-Proxy
===================================

Background
----------

This project was inspired by the following article. 

[Docker compose : Nginx reverse proxy with multiple containers](http://www.bogotobogo.com/DevOps/Docker/Docker-Compose-Nginx-Reverse-Proxy-Multiple-Containers.php) 

This example is based upon the github repo referneced in that article: https://github.com/Einsteinish/Docker-compose-Nginx-Reverse-Proxy

Purpose
-------

The purpose of this project is to have an example of something we would 
like to map into an openshift template. This example is greatly 
simplified from what we would like to acheive. The communication pattern exemplifies what we would like to acheive in our openshift deployment. 

In this example, there are three containers:
- **reverseproxy**

    this is the ingress point for all three containers. The reverseproxy container forwards traffic destined for the server-a and server-b endpoints.
- **server-a**

    This is a container that may only be reached via the /server-a context path routed through the **reverseproxy** container. 

- **server-b**

    This is a container that may only be reached via the /server-b context path routed through the **reverseproxy** container.

Each container above has been configured with a custom HTML page encoding the container name in the title. When the three containers are brought up, and queried with curl, this is the result. 

```
$ docker-compose up  -d
docker-compose-nginx-reverse-proxy_server-a_1 is up-to-date
docker-compose-nginx-reverse-proxy_server-b_1 is up-to-date
docker-compose-nginx-reverse-proxy_reverseproxy_1 is up-to-date

$ curl -s http://localhost | grep "<title>"
<title>Welcome to reverseproxy!</title>

$ curl -s http://localhost/server-a | grep "<title>"
<title>Welcome to Server A!</title>

$ curl -s http://localhost/server-b | grep "<title>"
<title>Welcome to Server B!</title>
```

We would like to create an equivilant openshift template to deploy containers in this sort of configuration. We are currently using Nginx as our reverseproxy, we would also consider either traefik or Apache HTTPD for this role. 

In our real deployment, containers in the server-a server-b role, would include Grafana, InfluxDB, and a few others that can be routed to with an HTTP reverse proxy. 