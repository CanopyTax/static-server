# Static Web Server
A Docker image for a static web server. 
This docker image is build on top of phusion base-image

It includes dependencies for python, node, and npm.
It works by using python Simple Http Server to server up your file.

All files must be mounted to /www

example Dockerfile:

    FROM canopytax/staticserver
    ADD ./ /www

example docker-compose.yml:

    web:
        image: canopytax/staticserver
        volumes: 
          - ./:/www
