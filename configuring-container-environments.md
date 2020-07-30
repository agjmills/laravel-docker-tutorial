# Configuring Container Environments
It's a good idea to build generic docker images, and then configure them within the context of 
your application. As an example, you might build a generic `elasticsearch` container, and allow the user of that
container to set the maximum memory limit, how many nodes are in the cluster, and so on.

This allows you to build fewer docker images, and therefore follows the DRY principle.

## Configuring using the `environment` attribute
If you look at the `elasticsearch` service, it has an `environment` attribute.

This sets a setting within the elasticsearch configuration. In this specific example, it stop elasticsearch from looking 
for other nodes to cluster with. 

## Configuring using an external file

If you look at the `database` service, it has an attribute named `env_file`. This works in the same way as the `environment`
attribute, it just configures the service inside the container in a certain way.

In this specific example: 

* It creates a database called `laravel` if it doesn't already exist
* It sets the MySQL root password to `secret`
* It sets the database port to `3306`
* It sets the database hostname to `database`

Using these settings, we should be able to connect to MySQL like this:

`docker exec -ti laravel-docker-tutorial_app_1 mysql -uroot -psecret -hdatabase laravel`

This runs the mysql command inside the `app` service.

## Notes

Environment variables are different for every service, but they are generally documented somewhere, it's best to look at 
https://hub.docker.com