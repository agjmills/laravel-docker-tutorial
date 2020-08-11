# Day to day development with Docker

This section contains a few things I've picked up after having developed Laravel projects using docker-compose on a day-to-day basis. 

## Learn the difference between `docker-compose stop` and `docker-compose down`
* `docker-compose down`: Stop and remove containers, networks, images, and volumes
* `docker-compose stop`: Stop services

Use `down` if you want to completly trash everything and start again, but use `stop` if you will be coming back to it at 
some point in the future.

## Go inside the container and have a look around
You can go inside any container by executing the following:

`docker exec -ti NAME_OF_CONTAINER bash` or `docker exec -ti NAME_OF_CONTAINER sh`  (if bash isnt installed)

## Use Laravel generators _outside_ of the container
If you run something like `php artisan make:migration add_users_table`, inside the container, it is run as `root`, which 
means that root then owns the file, and you'll have to `sudo chown -R your_user:your_user ./application/database/migrations`
to change the owner.

If you run it outside of the container, it'll create it with you as the owner instead.

## Execute Laravel & Artisan commands _inside_ the container
If you need to run the migrations, you _have_ to do it inside the container:

`docker exec -ti laravel-docker-tutorial_app_1 php artisan migrate`

Otherwise the application won't be able to resolve the hostname `database` for the MySQL server.

## Trash everything and rebuild from scratch
Sometimes, you break everything, or you just _really_ have to rebuild everything from scratch as it's not working properly.
Here are a few useful commands:

* `docker-compose up -d --force-recreate` Recreate containers even if their configuration and image haven't changed.
* `docker-compose up -d --force-recreate` Build images before starting containers.
    
## XDebug
When built, the app container image within this repository will have xdebug installed (if `WITH_XDEBUG=true` is defined within 
the `app.build.args` attribute inside `docker-compose.yml`).

By default, this will attempt to connect to `10.0.1.41` on port `9000`. You can change this by editing the `docker.env` file. Change the
IP address defined here to your local IP address, and [setup your IDE to accept connections on port 9000](https://dev.to/oranges13/phpstorm-xdebug-alpine-on-docker-13ff#setting-up-phpstorm). 