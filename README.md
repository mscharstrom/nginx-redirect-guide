<b>Redirect domain to your own NGINX web server and setup SSL.</b>
<br><br>
This guide assumes that you have already setup NGINX, configured nginx.conf and have a website that you want to redirect to. We are doing this redirection in our NGINX server and not on an application level.
<br><br>
Other prerequisites:<br>
- [x] You've set an "A" (HTTP) redirect from your web hotel to your own public IP of the webserver.
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
https://www.example.com &nbsp;&nbsp;&nbsp;HTTPS + "www"<br><br>



<b>I don't care, just show me the code!</b><br>
Here are the bits and bops that you need to add to your example.com.conf file. If this is unclear, please check my own example file example.com.conf in this repository for further details. <br>
<br>
```
  server {
  listen 80;
  server_name example.com www.example.com;
  return 301 https://example.com$request_uri;
}

server {
  listen 443 ssl;
  server_name example.com www.example.com;

  # ssl configuration
  ssl on;
  ssl_certificate /path/to/certificate.crt;
  ssl_certificate_key /path/to/private.key;

  if ($http_host = www.example.com) {
    return 301 https://example.com$request_uri;
  }
}
```
