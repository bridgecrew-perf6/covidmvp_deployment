# covidmvp_deployment
This  is a sample deployment of the [COVID MVP](https://github.com/cidgoh/COVID-MVP) 
using Docker, Nginx and a Linux server, with customizable header and footer.
# Deployment Steps

### step 1

Make sure [NGINX](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) is install on your system

- ``sudo nano /etc/nginx/sites-available/default``
- copy the below configuration to your into the above file and save 
``
  server {
    server_name  <domain or IP>;

    location / {
      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;
      proxy_pass <covid mvp local url: default 127.0.0.1:8050>;
      proxy_read_timeout  90;
     }

}

``
  
- ``sudo nginx -t`` verify configuration has no error.
- ``sudo systemctl restart nginx``

### step 2

After Cloning this repository to your system follow steps below.

- ``sudo docker-compose -f production.yml build``
- ``sudo docker-compose -f production.yml up``

### step 3 securing with ssl:
Follow the steps at [certbot](https://certbot.eff.org/instructions?ws=nginx&os=ubuntuother) to install cerbot for nginx on ubuntu and 
assign the ssl certificate.


### step 4 customer header setup.
This requires a server call via html Iframe into your already existing website.
Add the below line to you code.
`<iframe id="fid" src="https://your-covidMVP-domain" title="covid mvp">
</iframe>`
