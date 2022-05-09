# covidmvp_deployment
This  is a sample deployment of the [COVID MVP](https://github.com/cidgoh/COVID-MVP) 
using Docker, Nginx and a Linux server, with customizable header and footer.
# Deployment Steps

### step 1 configure nginx

Make sure [NGINX](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) is install on your system

- ``sudo nano /etc/nginx/sites-available/default``
- copy the below configuration to your into the above file and save. 
```
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

```
  
- ``sudo nginx -t`` verify configuration has no error.
- ``sudo systemctl restart nginx``

### step 2 run docker build and run docker image

After Cloning this repository to your system follow steps below.

- ``sudo docker-compose -f production.yml build``
- ``sudo docker-compose -f production.yml up. (this can be setup with supervisor. check step 5)``

### step 3 securing with ssl:
Follow the steps at [certbot](https://certbot.eff.org/instructions?ws=nginx&os=ubuntuother) to install cerbot for nginx on ubuntu and 
assign the ssl certificate.


### step 4 customer header setup.
This requires a server call via html Iframe into your already existing website.
Add the below line to you code.
``<iframe id="fid" src="https://your-covidMVP-domain" title="covid mvp">
</iframe>``

### step 5 supervisor setup

- update distrubution
``> sudo apt-get update -y``
- install supervisor
``> sudo apt-get install supervisor -y``
- check version
``> supervisord -v``
-check status
``systemctl status supervisor``
-  create covid mvp supervisor config file
``sudo nano /etc/supervisor/conf.d/covidmvp.conf``
- Add below text to config file. 
```[program:covidmvp]
    command=sudo docker-compose -f <path to production.yml file> up
    autostart=true
    autorestart=true
    startretries=5
    numprocs=1
    startsecs=0
    process_name=%(program_name)s_%(process_num)02d
    stderr_logfile=/var/log/supervisor/%(program_name)s_stderr.log
    stderr_logfile_maxbytes=10MB
    stdout_logfile=/var/log/supervisor/%(program_name)s_stdout.log
    stdout_logfile_maxbytes=10MB```
- Inform supervisor of a new config file
``sudo supervisorctl reread``
- Run the new configuration
``sudo supervisorctl update``
- check if configuration is running
``sudo supervisorctl``
