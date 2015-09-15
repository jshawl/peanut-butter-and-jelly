# Deployment

## Start with a working application

    $ git clone git@github.com:ga-dc/

## Create a Droplet

1. Name your Droplet (anything)
2. Select Size ($5)
3. Select region (New york 3)
4. Select Image (Ubunutu 14.04 x64)
5. Add SSH Key
   
    $ cat ~/.ssh/id_rsa.pub | pbcopy

## Log into Droplet

    $ ssh root@your-droplets-ip

Create a new secure password.

## Install Git and node

    $ sudo apt-get update
    $ sudo apt-get install git nodejs npm
    $ sudo ln -s /usr/bin/nodejs /usr/bin/node

Verify with `which git` `which node`

## Install Postgresql

    $ sudo apt-get install postgresql postgresql-contrib
    $ sudo -u postgres psql postgres

## Install a webserver

   $ sudo apt-get install nginx

Verify it works by visiting http://your-droplets-ip-address/ in a browser

## Clone the app to VPS

Typically you would create a user account to handle deploys and permissions. In the interest of time,
we'll continue using the root user.

>Q: Why might this be a bad idea?

    $ mkdir /var/www
    $ cd /var/www
    $ git clone https://github.com/ga-dc/tunr_node_oojs.git
    $ cd tunr_node_oojs
    $ npm install
    $ sudo -u postgres createuser <local username>
    $ sudo su - postgres
    $ ALTER ROLE username WITH LOGIN;

    http://stackoverflow.com/q/20703887/850825

    $ createdb
    $ node db/schema.js
    $ node db/seeds.js
    $ node app.js

Visit http://your-ip-address:3000/

Something in the JS is broken. It's referencing local host

Replace every localhost with a relative path.

## Set up domain and proxy server

On your local machine (not on droplet)

    $ sudo vim /etc/hosts

put in

    your-ip-address    tunr.com

eventually do the same thing on the server

Check out tunr.com:3000

## Configure a reverse proxy!

Back in the droplet

    $ cd /etc/nginx/sites-enabled/
    $ sudo vim tunr.com

```
server{
  server_name tunr.com;
  listen 80;
  root /path/to/express/public/folder;
  location / {
    try_files $uri @tunr;
  }
  location @tunr {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header Host $host;
    proxy_pass http://127.0.0.1:3000;
  }
}

```

Restart nginx

    $ sudo service nginx restart
    $ node /path/to/app/app.js


You could fix bad gateway error by going back to app
and node app.js


but..... there's a better way

    $ npm install -g forever
    $ cd /path/to/app
    $ forever start app.js

