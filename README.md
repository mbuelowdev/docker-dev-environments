# Docker Development Images

### Available images

* nginx-fpm-php7.3-composer2.4.3
* nginx-fpm-php8.1-composer2.4.3

### Customizing the image

It is recommended to create your local user in the development environment.
Otherwise the container will create files as root. Simply add your user
to the top of the Dockerfile. Somewhere here:

```
# Add your local user here
RUN useradd -u 1000 myuser
```

You can use the `id` command to determine your numeric user id.

### Building the image

1. Navigate into the directory
2. Customize the image beforehand as described in
[Customizing the image](#customizing-the-image).
3. Execute `docker build -t $(basename $PWD) .`

### How to create a new project

1. Go into the folder where you want to create your new project. We'll use
`/projects` in this example.
2. Select your desired development environment by selecting a docker image
from [Available Images](#available-images). It is recommended to customize
the images beforehand by adding your local user to them (e.g. `myuser`).
3. Open a shell by executing the following command:
`docker container run --rm -v /projects:/creator -u myuser -w /creator -it your-image-name "/bin/bash"`
This will open a fresh container at /creator where you can run your
installation commands.
4. Afterwards you can close the container, navigate into your new project
directory and copy the `docker-compose.yaml` template to it.
5. Modify the compose file to your needs and start it. You can use the same
image from the creation phase. However now you shouldn't mount to the
`/creator` directory but rather to the actual runtime directory.

### Minimal docker-compose.yaml

```
version: '3'

services:
    myproject:
    container_name: myproject
    image: your-image-name
        volumes:
            - ./:/var/www/html/phpsite
        ports:
            - "80:80"
        expose:
            - "80"
```
