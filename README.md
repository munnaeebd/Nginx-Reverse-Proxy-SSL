# Nginx-Reverse-Proxy-SSL
Nginx-Reverse-Proxy-SSL

```
mkdir /etc/ssl/private
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
openssl dhparam -out /etc/ssl/private/dhparam.pem 2048
mv /etc/ssl/certs/nginx-selfsigned.crt /etc/ssl/private/
```

Ghost Site
```
mkdir -p /srv/ghost/content

docker run --restart always -d --name ghost-blog \
-v /srv/ghost/content:/var/lib/ghost/content \
-e url=http://ghost-blog \
-e mail__transport="SMTP" \
-e mail__from="hejbul.tawhid@brilliant.com.bd" \
-e mail__options__service="SMTP" \
-e mail__options__host="mta.brilliant.com.bd" \
-e mail__options__port="587" \
-e mail__options__auth__user="hejbul.tawhid@brilliant.com.bd" \
-e mail__options__auth__pass="" \
ghost
```
# reload from inside nginx container 
nginx -s reload


# Nginx-Reverse-Proxy

~~~
docker build -t nginx-proxy:20 .
docker run -d --name reverse-proxy-ssl -p 443:443 -p 80:80  --link ghost-blog nginx-proxy:20
~~~


