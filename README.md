<b>Redirect domain to your own NGINX web server and setup SSL.</b>
<br><br>
This guide assumes that you have already setup NGINX, configured nginx.conf and have a website that you want to redirect to.
<br><br>
Other prerequisites:<br>
- [x] You've set an "A" redirect from your web hotel to your own public IP of the webserver.
- [x] Your webserver is up and running and you have an example.com website on it (for eg. in /var/www/example.com/).
- [x] Configure nginx.conf to include "/etc/nginx/conf.d/*.conf" and not "/etc/nginx/sites-enabled*;".

<b>Why even bother?</b><br>
Without SSL, your website will show insecure to the visitors. Therefore, using an SSL-encrypted connection for safety, accessibility or PCI compliance reasons is necessary. It becomes very important to redirect from HTTP to HTTPS and it is more or less default nowadays to use HTTPS.<br>
<br>
Previously, one only had to redirect from an www.example.com to example.com and/or vice-versa. Since the implementation of HTTPS we now have 4 options to enter a website that we have to take in considiration.<br>
<br>
http://example.com &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HTTP<br>
https://example.com &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HTTPS<br>
http://www.example.com &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HTTP + "www"<br>
https://www.example.com &nbsp;&nbsp;&nbsp;HTTPS + "www"<br>
