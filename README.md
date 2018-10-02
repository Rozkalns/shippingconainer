# shippingconainer

### Build image
```bash
$ docker build -t wrong-docker/app:latest \
  -f docker/app/Dockerfile \
  docker/app
```

### Run container
```bash
$ docker run -d --rm -p 8080:80 -v $(pwd)/app:/var/www/html wrong-docker/app:latest
```
