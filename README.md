Assignment 1
Part#4

viru@viru:~$ docker network create --driver bridge my_network
5d363165c9b380481a01d37425eb89cd232de396803d3e47800087fd282f9365

viru@viru:~$ docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
a880f41d2931   bridge       bridge    local
f25404887056   host         host      local
5d363165c9b3   my_network   bridge    local
20a2ce0bcebf   none         null      local

viru@viru:~$ docker run -d --name nginx_container --network my_network -p 8080:80 nginx
a6b38d3a47adbdc3e876c75c91c46d21de69b6f081548fb756b127ede8e67df5
viru@viru:~$ docker inspect nginx_container --format '{{json .NetworkSettings.Networks}}'
{"my_network":{"IPAMConfig":null,"Links":null,"Aliases":["a6b38d3a47ad"],"NetworkID":"5d363165c9b380481a01d37425eb89cd232de396803d3e47800087fd282f9365","EndpointID":"cb27a4201f75166aa67ae68abae743f3891c5b5873577b55a9836248c0edc316","Gateway":"172.18.0.1","IPAddress":"172.18.0.2","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"MacAddress":"02:42:ac:12:00:02","DriverOpts":null}}

viru@viru:~$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>


viru@viru:~$ docker run -d --name httpd_container --network my_network -p 8081:80 httpd
207107c1e46034af3b60e03c8ec187d02936971d186255612fb7effdc39e7d2e

viru@viru:~$ curl http://localhost:8081
<html><body><h1>It works!</h1></body></html>


viru@viru:~$ docker network inspect my_network
[
    {
        "Name": "my_network",
        "Id": "5d363165c9b380481a01d37425eb89cd232de396803d3e47800087fd282f9365",
        "Created": "2023-03-26T23:31:11.452766485+05:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
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
            "207107c1e46034af3b60e03c8ec187d02936971d186255612fb7effdc39e7d2e": {
                "Name": "httpd_container",
                "EndpointID": "c19bf25a33c4f051c81585f9eabd827ff9670379dd65b78733793d15d4ce86bd",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "a6b38d3a47adbdc3e876c75c91c46d21de69b6f081548fb756b127ede8e67df5": {
                "Name": "nginx_container",
                "EndpointID": "cb27a4201f75166aa67ae68abae743f3891c5b5873577b55a9836248c0edc316",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]


viru@viru:~$ docker stop nginx_container
nginx_container

viru@viru:~$ docker rm nginx_container
nginx_container


viru@viru:~$ docker run -d --name nginx_container_2 --network my_network -p 8082:80 nginx
755ca35c6a8651816a0259c202311f689f5e11af5bbf9af00a19036b89760407


viru@viru:~$ curl http://localhost:8082
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>


viru@viru:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                   NAMES
755ca35c6a86   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8082->80/tcp, :::8082->80/tcp   nginx_container_2
207107c1e460   httpd     "httpd-foreground"       6 minutes ago        Up 6 minutes        0.0.0.0:8081->80/tcp, :::8081->80/tcp   httpd_container


viru@viru:~$ docker stop nginx_container_2
nginx_container_2

viru@viru:~$ docker rm nginx_container_2
nginx_container_2

viru@viru:~$ docker stop httpd_container
httpd_container

viru@viru:~$ docker rm httpd_container
httpd_container

viru@viru:~$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

viru@viru:~$ docker network rm my_network
my_network

viru@viru:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a880f41d2931   bridge    bridge    local
f25404887056   host      host      local
20a2ce0bcebf   none      null      local
