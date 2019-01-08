<b>Redirect HTTP to HTTPS on your own NGINX web server and setup SSL.</b>
<br><br>
This guide assumes that you have already setup NGINX, configured nginx.conf and have DNS redirected to your server from your web hotel. The redirection in this guide will be server-side and not on an application level. I have written/compiled this guide from multiple sources online and we are focusing only on an linux environment (I have found that many Raspberry Pi users seem to be asking questinos about this).
<br><br>
Other prerequisites:<br>
- [x] You've set an "A" (HTTP) redirect from your web hotel to your own public IP of the webserver.
- [x] Your webserver is up and running and you have an example.com website on it (for eg. in /var/www/example.com/).

<b>Why even bother?</b><br>
Without SSL, your website will show insecure to the visitors. SSL encrypts requests made between a web server and a visitors browser and it also make requests and responsens happen in a way so that they can't be intercepted. The certificates that is being authenticated makes sure that the certificate isn't intended for a different domain, if so the user will get alerted and the browser will most likely block the request. Users today expect a secure and private online experience when using a website. It is more or less default nowadays to use TLS/SSL (HTTPS).<br>

Google has written a [great article](https://support.google.com/webmasters/answer/6073543?hl=en) on this matter with a handful of useful tips. In short it provides three key layers of protection: <br>
1. Encryption — encrypting the exchanged data to keep it secure from eavesdroppers. That means that while the user is browsing a website, nobody can "listen" to their conversations, track their activities across multiple pages, or steal their information.<br>
2. Data integrity — data cannot be modified or corrupted during transfer, intentionally or otherwise, without being detected<br>
3. Authentication — proves that your users communicate with the intended website. It protects against man-in-the-middle attacks and builds user trust, which translates into other business benefits.<br>

Worth noting is that previously one only had to redirect from an www.example.com to example.com and/or vice-versa. Since the implementation of HTTPS we now have 4 options to enter a website that we have to take in consideration.<br>
<br>
http://example.com &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HTTP<br>
https://example.com &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HTTPS<br>
http://www.example.com &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HTTP + "www"<br>
https://www.example.com &nbsp;&nbsp;&nbsp;HTTPS + "www"<br>

But fear not, the process is fairly simple.<br>

<b>Let's Encrypt!</b></br>
To be able to redirect your HTTP to HTTPS you will need valid certificates. To get this we are going to use Let's Encrypt which is a free service that allows you to create SSL certificates. Keep in mind that these certificates are kinda short lifed. <br>

Start with making sure that you are all up to date:<br>
```
sudo apt-get update
sudo apt get upgrade
```
<br>

Next install Let's Encrypt:<br>
```
sudo apt-get install letsencrypt
````

Now let's create some free SSL certifications! Make sure to change out example.com to your specific folder and the file path /var/www/ if your web app is located elsewhere:<br>
```
letsencrypt certonly --webroot -w /var/www/example.com -d example.com -d www.example.com
```

<b>Certificate done, whats next?!</b><br>
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
