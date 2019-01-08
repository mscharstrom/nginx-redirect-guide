<b>Redirect domain to your own NGINX web server and setup SSL.</b>
<br><br>
This guide assumes that you have already setup NGINX, configured nginx.conf and have a website that you want to redirect to.
<br><br>
Other prerequisites:<br>
- [x] You've set an "A" redirect from your web hotel to your own public IP of the webserver.
- [x] Your webserver is up and running and you have an example.com website on it (for eg. in /var/www/example.com/).
- [x] Configure nginx.conf to include "/etc/nginx/conf.d/*.conf" and not "/etc/nginx/sites-enabled*;".
