# nginx-cdn
- [Helpful guide](https://dev.to/shashankpai/building-a-simple-cdn-with-nginx-and-docker-a-step-by-step-guide-lkg).

To be used as cdn


## Copy the existing nginx.conf file
```sh
docker run --rm --entrypoint=cat nginx /etc/nginx/nginx.conf > nginx.conf
```

## Run 

```sh
mkdir -p html

docker run --rm --name nginx-cdn \
    -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
    -v $(pwd)/html:/usr/share/nginx/html:ro \
    -p 8080:80 \
    nginx:latest
```

--- 

Origin & edge server configuration are detailed in docker-compose.yml

```sh
#Start
docker compose up
curl -v localhost:8080/

#Stop
docker compose down
```

Result to observe with `curl`.

1st call

```sh
url -v -X GET localhost:8080
Note: Unnecessary use of -X or --request, GET is already inferred.
* Host localhost:8080 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:8080...
* Connected to localhost (::1) port 8080
> GET / HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/8.5.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx/1.29.1
< Date: Thu, 25 Sep 2025 20:39:06 GMT
< Content-Type: text/html
< Content-Length: 72
< Connection: keep-alive
< Last-Modified: Thu, 18 Sep 2025 21:57:05 GMT
< ETag: "68cc8031-48"
< X-Proxy-Cache: MISS
< Accept-Ranges: bytes

```

2nd call

```sh
curl -v -X GET localhost:8080
Note: Unnecessary use of -X or --request, GET is already inferred.
* Host localhost:8080 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:8080...
* Connected to localhost (::1) port 8080
> GET / HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/8.5.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx/1.29.1
< Date: Thu, 25 Sep 2025 20:39:11 GMT
< Content-Type: text/html
< Content-Length: 72
< Connection: keep-alive
< Last-Modified: Thu, 18 Sep 2025 21:57:05 GMT
< ETag: "68cc8031-48"
< X-Proxy-Cache: HIT
< Accept-Ranges: bytes

```