# covidmvp_deployment
This  is a sample deployment of the [COVID MVP](https://github.com/cidgoh/COVID-MVP) 
using Docker, Nginx, with customizable header and footer.
# Deploment Steps

### step 1

make sure [NGINX](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) is install on your system

- ``sudo nano /etc/nginx/sites-available/default``
- copy the below configuration to your into the above file and save 
`` ``
  
- ``sudo nginx -t`` verify configuration has no error.
- ``sudo systemctl restart nginx``

### step 2

After Cloning this repository to your system follow steps below.

- ``sudo docker-compose -f production.yml build``
- ``sudo docker-compose -f production.yml up``

