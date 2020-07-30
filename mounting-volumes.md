# Mounting the application into the container

You will see in certain services within `docker-compose.yml`, that they have a `volumes` attribute.

This defines what files or folders will be mounted inside the container. They are _mounted_ and not _copied_, so 
any changes you make outside of the container will be reflected inside the container, too.

Looking at the `app` service:

```
    volumes:
      - ./application:/var/www
``` 

This mounts the application directory (which contains Laravel) to `/var/www` inside the container.