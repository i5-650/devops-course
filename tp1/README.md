# TP1

## Questions

### Question 1 : Document your database container essentials: commands and Dockerfile.
The commands and so are [here](db/commands.md)

### Question 2 : Why do we need a multistage build? And explain each step of this dockerfile.
We use a multistage build in order to partition the build and the run steps.
The running environment doesn't need the same tools as the building environment.
It's more secure to use a multistage build because the final image is lighter and doesn't contain any unnecessary tools nor sources files.

### Question 3 : Document docker-compose most important commands
The most important commands are:
- `docker compose up` : starts the services with the `docker-compose.yaml` file if there is no `docker-compose.override.yaml` file. If there is one, it will use it and override the `docker-compose.yaml` file.
- You can use the `-f` to specify the file to use. For instance `docker-compose -f docker-compose.yaml up` to run our dockers in a non-dev environment.
- Option `-d` to run the services in the background.

### Document your docker-compose file.
The docker-compose file is [here](docker-compose.yaml)
Yet for the development environment, we use the `docker-compose.override.yaml` file. Because it allows you to access the database from the host machine or via adminer.


Every service is linked to the same network, so they can communicate with each other. (`hehe-network`)


Most of them require the `.env` file to be set up. It contains the environment variables for the services (password, username, etc). Like this, we can use the same `docker-compose.yaml` file for every environment. And have it in the git without any sensitive information.

### Question 5 : Document your publication commands and published images in dockerhub.

In order to publish, you need to be logged in to the docker hub. You do this using:
```bash
docker login
```
(everything is interactive, you need to enter your username and password.)


Then you need the docker images to be built. You do this using:
```bash
docker built -t http-server-tp1 /path/to/http-server-tp1/dockerfile-folder
```
*Note: You want the folder where the `Dockerfile` is located, not the `Dockerfile` itself.*
In this case `http-server-tp1` is the name of the image.

Then you need to tag the image with your docker hub username and the name of the image you want to publish. It's also a good practice to tag the image with a version number.
```bash
docker tag http-server-tp1 username/http-server-tp1:1.0
```
Then you can push the image to the docker hub.
```bash
docker push username/http-server-tp1:1.0
```

## Httpd Conf
The httpd conf is [here](http-server-tp1/httpd.conf).
Where the reverse proxy and other configurations are isn't the place they should be. There is sections for the reverse proxy and the virtual hosts.

But for debugging purposes, I put everything at the end of the file.

## End
The images are available on the docker hub: https://hub.docker.com/u/b0x550xaat

*Note*: `external : true` doesnt create the network if it doesn't exist.
