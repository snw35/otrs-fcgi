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

## About my images

All of my containers follow these main guidelines:

 * __Follow best practice.__ Adhere as closely as possible to the [official docker library image guidelines.](https://github.com/docker-library/official-images)
 * __No version-dependent scripts.__ No custom scripts or glue code that would need to be updated alongside the hosted application.
 * __Small and simple.__ Based on official Alpine Linux with as minimal Dockerfiles and images as possible.
 * __One process, one container.__ No process supervisor daemons or hacks. Allow the container runtime to have full visibility of each running process.
 * __Disposable and immutable.__ Strict separation of user and application data means that any container can be stopped and replaced without affecting the configuration state of the application.
 * __Security focused.__ No docker socket bind mounts, ever. All processes are run by a dedicated application user with minimal permissions where possible.
 * __True to the application.__ The application's defaults are not altered in any way, and no additions or subtractions are made to functionality.

If you like these guidelines, then please check out my other images here or on Dockerhub.

[1]: https://github.com/snw35/otrs-docker

***

 * [Travis CI: ![Build Status](https://travis-ci.org/snw35/otrs-fcgi.svg?branch=master)](https://travis-ci.org/snw35/otrs-fcgi)
