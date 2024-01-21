1- 
docker network create my_network
docker network ls
626b14934cc1   my_network            bridge    local

-------------------------------------------------------------------------------------

2-
docker run -d -p 8000:80 --name nginx_container --network=my_network nginx

docker ps 

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
ba9b86bc5d5c   nginx     "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:8000->80/tcp, :::8000->80/tcp   nginx_container

------------------------------------------------------------------------------------

3- Nginx is available on localhost:8000

Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.

-------------------------------------------------------------------------------------

4- docker run -d -p 8081:80 --name httpd_container --network=my_network httpd

docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
5d466c088049   httpd     "httpd-foreground"       4 seconds ago   Up 2 seconds   0.0.0.0:8081->80/tcp, :::8081->80/tcp   httpd_container
-----------------------------------------------------------------------------------------

5- httpd is available on port 8081 with following output:

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
5d466c088049   httpd     "httpd-foreground"       4 seconds ago   Up 2 seconds   0.0.0.0:8081->80/tcp, :::8081->80/tcp   httpd_container

------------------------------------------------------------------------------------------

6- docker network inspect my_network

[
    {
        "Name": "my_network",
        "Id": "626b14934cc18973ebb226bed83f5fcca6d798941faee2c029b59fc719ab7aea",
        "Created": "2024-01-21T11:11:38.048235773+05:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.20.0.0/16",
                    "Gateway": "172.20.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "5d466c088049c450cca753c72fe7af862b7d24f75ca46e9a7de6076e5e309b7a": {
                "Name": "httpd_container",
                "EndpointID": "1a971ee25d8ba5f8006cc463770f0f42ba5ca0b8774f8a07f0bd95d5d1941978",
                "MacAddress": "02:42:ac:14:00:03",
                "IPv4Address": "172.20.0.3/16",
                "IPv6Address": ""
            },
            "ba9b86bc5d5ceefd81446a08c29bb889f05b8a753e68809a686aa7cc87f62434": {
                "Name": "nginx_container",
                "EndpointID": "d7e8323f3784f9271af0f9baac3b8e5292a605c962a229eb06e5c11b211c56c2",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

----------------------------------------------------------------------------------------------------------

7 - docker run -d -p 8082:80 --name nginx_container_2 --network=my_network nginx

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
5f9c7d3567f0   nginx     "/docker-entrypoint.…"   3 seconds ago    Up 2 seconds    0.0.0.0:8082->80/tcp, :::8082->80/tcp   nginx_container_2

----------------------------------------------------------------------------------------------------------

8 - Nginx available on 8082

Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.

------------------------------------------------------------------------------------------------------------

9 - 
docker stop ba9b86bc5d5c
docker rm ba9b86bc5d5c

------------------------------------------------------------------------------------------------------------

10 - docker container ls

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
5f9c7d3567f0   nginx     "/docker-entrypoint.…"   3 minutes ago    Up 3 minutes    0.0.0.0:8082->80/tcp, :::8082->80/tcp   nginx_container_2
5d466c088049   httpd     "httpd-foreground"       19 minutes ago   Up 19 minutes   0.0.0.0:8081->80/tcp, :::8081->80/tcp   httpd_container

---------------------------------------------------------------------------------------------------------------

11 - 
docker stop $(docker ps -q)
docker rm $(docker ps -a -q)

---------------------------------------------------------------------------------------------------------------

12 - 

docker network rm my_network
my_network



