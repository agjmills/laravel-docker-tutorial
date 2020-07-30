# How `docker-compose.yml` works

A `docker-compose.yml` file can be seen as a YAML description of your applications stack.

## Connecting to each service

So, with the `docker-compose.yml` in this project, you can see several "services":

* web (nginx)
* app (php-fpm)
* redis
* elasticsearch
* kibana
* database (mysql)
* mailhog

Each of these runs in a separate container, and is addressable via the name of the service from within another container.

So, if you wanted to connect to the container called `elasticsearch` from the container called `app`, you might do something 
within the app container like this: `curl http://elasticsearch:9200`.

You'll notice, that the elasticsearch service doesn't have a `ports` attribute - this means the port inside the container
isn't bound to a port on the host system. 

But, if you look at the `web` service, that has a port definition of `127.0.0.1:80:80`, which means that `127.0.0.1`, port `80`, is bound to port `80` inside the container.
So we should be able to open our web browser and navigate to `http://127.0.0.1` and we will see the Laravel default screen.

I have deliberately included `.env` witin the application directory so that you can see how to configure Laravel to connect
to the various services.

## Service links

Some of the services have a linked attribute - these are basically showing the dependencies between the services,
and it gives `docker-compose` an idea of which order to start certain services in. 

Going from the top of the file, we can see that `web` is dependent on `app`, `app` is dependent on `database`, `redis` and `elasticsearch`. So 
they'll be started in roughly that order.   