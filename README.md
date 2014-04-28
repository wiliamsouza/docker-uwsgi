docker-uwsgi
------------

Docker uwsgi server generic image source. This is based on `ubuntu:12.04` image.

Image
-----

You can `pull` a ready to use image from Docker
[index](https://index.docker.io/u/wiliamsouza/) running:

```
$ docker.io pull wiliamsouza/uwsgi
```

Or build this repository:

```
$ git clone https://github.com/wiliamsouza/docker-uwsgi.git
$ cd docker-uwsgi/
$ docker.io build -t wiliamsouza/uwsgi .
```

Change `wiliamsouza/uwsgi` to your Docker index username.

Container
---------

This image uses volumes and environment variables to control the uwsgi server
configuration.

Volumes:

* `/var/log/uwsgi`: Access log from the container using it.
* `/etc/uwsgi`: Change server configurations using it.
* `/srv`: App code goes here.

You pass with `-v` docker option. Don't forget to use absolute path here.

Environment variable:

* `DJANGO_SETTINGS_MODULE`: Must point to Django settings like `myproject.settings`.

You pass with `-e` docker option.

This source is shiped with a minimal Django project in `volumes/project` to use it
for test you need to create and `virtualenv` in `volumes/virtualenv` and run `pip install requirements.txt`
in order to install its dependencies.

Shell access:

```
$ docker.io run -p 8000:8000 -i \
-v `pwd`/volumes/virtualenv:/srv/virtualenv \
-v `pwd`/volumes/project:/srv/project \
-v `pwd`/volumes/log:/var/log/uwsgi \
-v `pwd`/volumes/etc:/etc/uwsgi \
-e DJANGO_SETTINGS_MODULE=myproject.settings \
-t wiliamsouza/uwsgi /bin/bash
```

The command above will start a container give you a shell. Don't
forget to start the service running the `startup &` script.

Usage:

```
$ docker.io run --name uwsgi -p 8000:8000 -d \
-v `pwd`/volumes/virtualenv:/srv/virtualenv \
-v `pwd`/volumes/project:/srv/project \
-v `pwd`/volumes/log:/var/log/uwsgi \
-v `pwd`/volumes/etc:/etc/uwsgi \
-e DJANGO_SETTINGS_MODULE=myproject.settings \
-t wiliamsouza/uwsgi
```

The command above will start a container and return its ID.
