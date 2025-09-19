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
