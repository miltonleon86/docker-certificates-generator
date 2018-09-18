# Description

This project is thought to avoid the pain of generating TLS|SSL certificates
in a docker environment. Your public and private keys will be generated 
inside a /docker-volumes folder that you can use to match the key for each 
of the containers if required.

### 1. How to start.
I'm using nginx proxi on top of all containers and redirecting to the container
nginx using env VIRTUAL_HOST, so you need to define it on composer and 
your own network.

```$bash
- VIRTUAL_HOST=domain.com,www.domain.com
```
```$bash
networks:
      - my_network
```

```$bash
networks:
  my_network:
    external:
      name: my_network_me
```
and the nginx.conf
```$bash
server_name domain.com www.domain.com;

```

### 2. start composer.
```$bash
docker-compose up -d
```

### 3. Run the certbot docker.
```$bash
networks:
sudo docker run -it --rm \
-v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /src/letsencrypt/letsencrypt-site:/data/letsencrypt \
-v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
certbot/certbot \
certonly --webroot \
--email mail@gmail.com --agree-tos --no-eff-email \
--webroot-path=/data/letsencrypt \
-d domain.com -d www.domain.com
```
### 4. Desired Response.

```$bash
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/domain.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/domain.com/privkey.pem
   Your cert will expire on 2018-12-17. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

you will have the certificates under /docker-volumes/etc/letsencrypt/live ready to use.
