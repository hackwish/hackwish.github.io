hackwish.github.io
==================

Docker Test Lab
---------------

```
docker run --rm \
--name mdweb \
-p 8000:80 \
-v ${PWD}:/usr/share/nginx/html \
nginx
```
