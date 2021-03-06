rodeo-desktop-ubuntu
====================

This image is a "apt-get install" of the Rodeo Python IDE.

# Getting the image

## Use the docker repository:

```
docker pull timcera/rodeo-desktop-ubuntu:latest
```

Required Manual Installation
----------------------------
To run a container create a shell script similar to below, perhaps called 
'rodeo', but you can call it anything you want.

```
#!/bin/sh

# Should be platform neutral - at least working on Linux and Windows
USER_NAME=`basename $HOME`

# HHHOME is used to pass the HOME directory of the user running rodeo
# and is used in "start.sh" to create the same user within the container.

# Users home is mounted as home
# --rm will remove the container as soon as it ends
docker run --rm \
    -i -t \
    -v ${HOME}:/home/${USER_NAME} \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e DISPLAY=unix$DISPLAY \
    -e HHHOME=${HOME} \
    timcera/rodeo-desktop-ubuntu:latest
```

Be sure to make the "rodeo" script (or whatever you called your script) an
executable.  

```
chmod a+x rodeo
```

The above is the content of rodeo so you can just

```
./rodeo
```

The "-v ${HOME}:/home/${USER_NAME}" option will mount your home directory in
the container.  If you have other mount points, add "-v" options as necessary.

Put into a directory listed in your PATH environment variable.

```
sudo cp rodeo /usr/local/bin
```

If rodeo crashes or hangs it might leave an orphan docker process running. If
you see the process with 

```
docker ps
```

Then run 

```
docker stop <process id or container name>
```

Else run 

```
docker ps -a
```

then

```
docker rm <process id or container name>
```

-----------

Tim Cera (tim@cerazone.net)
