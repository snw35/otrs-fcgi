# otrs-fcgi
OTRS perl fast-CGI docker container based on Alpine Linux.

This is the primary container image of my [otrs-docker][1] deployment. It holds the OTRS installation files, creates the needed volumes, and installs all of OTRS' perl dependencies. It presents a perl fast CGI daemon (fcgi wrap running under spawnfcgi) on port 9000, which is designed to be proxied upstream by Nginx.

## How to deploy

Please see my main [otrs-docker][1] repository for full instructions and the docker-compose file that automates the deployment of these images.

If you would like to run this image manually for testing purposes, then this docker run command will start it in the same way as the docker-compose file:
```
docker run -dt --restart unless-stopped --name otrs-fcgi --mount source=otrs-config,target=/data --mount source=otrs-dir,target=/opt/otrs --network otrs-backend snw35/otrs-fcgi:latest
```
Note that you will still need a database and nginx webserver configured to proxy CGI requests upstream to this container to actually access the OTRS web interface. The docker-compose.yml file in the [otrs-docker][1] repository will automate this for you.

***

 * [Travis CI: ![Build Status](https://travis-ci.org/snw35/otrs-fcgi.svg?branch=master)](https://travis-ci.org/snw35/otrs-fcgi)
