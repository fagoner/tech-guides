# Before start typing

The following guide was tested on DigitalOcean Droplet with `ubuntu/20.04`.
The `example.com / www.example.com` value should be updated  with a domain previously acquired

Credits / Source: 
- [Nodejs Deploy | DigitalOcean, Nginx, PM2, y SSL con Let's Encrypt (Certbot)](https://www.youtube.com/watch?v=6qR_EpxadMo)


# Nginx part 
### Install nginx
`sudo apt-get install nginx -y`

### Create nginx/sites-available/example.com

`vim /etc/nginx/sites-available/example.com`

Fill the following data (use your domain and port)

```
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

### Check config
`nginx -t`

### Create a link for nginx/sites-available/example.com

`ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/example.com`

### Restart service

`sudo service nginx restart`

Check the `ip/domain`

# Certbot part

### Install

`sudo snap install core; sudo snap refresh core`

`sudo snap install --classic certbot`
### Apply certs

`sudo certbot --nginx -d example.com -d www.example.com`

# UFW part 

### Check status
`sudo ufw status`

### Enable
`sudo ufw enable`

### https / ssh
`sudo ufw allow https`

`sudo ufw allow ssh`

### Reload after configurations to change
`sudo ufw reload`
