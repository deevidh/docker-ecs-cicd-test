# Hello world nginx container

Config files to build a basic "hello world" nginx Docker container

How to build manually:

```bash
docker build -t hello-world .
```

How to run (this maps port 80 on the container to port 8080 on the host):

```bash
docker run -p 8080:80 hello-world
```

To test the container:

```bash
curl -I 127.0.0.1:8080
```

```text
HTTP/1.1 200 OK
Server: nginx/1.21.6
Date: Tue, 01 Feb 2022 18:38:21 GMT
Content-Type: text/html
Connection: keep-alive
Expires: Tue, 01 Feb 2022 18:38:20 GMT
Cache-Control: no-cache
```
