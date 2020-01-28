## Example 1: hello world

It's time to run your first container:

```bash
docker run ubuntu /bin/echo 'Hello world'
```

Console output:

```
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
6b98dfc16071: Pull complete
4001a1209541: Pull complete
6319fc68c576: Pull complete
b24603670dc3: Pull complete
97f170c87c6f: Pull complete
Digest: sha256:5f4bdc3467537cbbe563e80db2c3ec95d548a9145d64453b06939c4592d67b6d
Status: Downloaded newer image for ubuntu:latest
Hello world
```

* **docker run** is a command to run a container.
* **ubuntu** is the image you run. For example, the Ubuntu operating system image. When you specify an image, Docker looks first for the image on your Docker host. If the image does not exist locally, then the image is pulled from the public image registry -- Docker Hub.
* **/bin/echo 'Hello world'** is the command that will run inside a new container. This container simply prints “Hello world” and stops the execution.

Let's try to create an interactive shell inside a Docker container:

```bash
docker run -i -t --rm ubuntu /bin/bash
```

* **-t** flag assigns a pseudo-tty or terminal inside the new container.
* **-i** flag allows you to make an interactive connection by grabbing the standard input (STDIN) of the container.
* **--rm** flag automatically removes the container when the process exits. By default, containers are not deleted. This container exists until we keep the shell session and terminates when we exit the session (like an SSH session with a remote server).

If you want to keep the container running after the end of the session, you need to daemonize it:

```bash
docker run --name daemon -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

* **--name daemon** assigns daemon name to a new container. If you don't specify a name explicitly, Docker will generate and assign it automatically.
* **-d** flag runs the container in the background (i.e., daemonizes it).

Let's see what containers we have at the moment:

```bash
docker ps -a
```

Console output:

```
CONTAINER ID  IMAGE   COMMAND                 CREATED             STATUS                         PORTS  NAMES
1fc8cee64ec2  ubuntu  "/bin/sh -c 'while..."  32 seconds ago      Up 30 seconds                         daemon
c006f1a02edf  ubuntu  "/bin/echo 'Hello ..."  About a minute ago  Exited (0) About a minute ago         gifted_nobel
```

* **docker ps** is a command to list containers.
* **-a** shows all containers (without -a flag ps will show only running containers).

The **ps** shows us that we have two containers:

* **gifted_nobel** (the name for this container was generated automatically – it will be different on your machine). It's the first container we created, the one that printed 'Hello world' once.
* **daemon** -- the third container we created, which runs as a daemon.

Note: there is no second container (the one with interactive shell) because we set the **--rm option**. As a result, this container is automatically deleted right after execution.

Let's check the logs and see what the daemon container is doing right now:

```bash
docker logs -f daemon
```

Console output:

```
...
hello world
hello world
hello world
```

* **docker logs** fetch the logs of a container.
* **-f** flag to follow the log output (works actually like **tail -f**).

Now let's stop the daemon container:

```bash
docker stop daemon
```

Make sure the container has stopped.

```bash
docker ps -a
```

Console output:

```
CONTAINER ID  IMAGE   COMMAND                 CREATED        STATUS                      PORTS  NAMES
1fc8cee64ec2  ubuntu  "/bin/sh -c 'while..."  5 minutes ago  Exited (137) 5 seconds ago         daemon
c006f1a02edf  ubuntu  "/bin/echo 'Hello ..."  6 minutes ago  Exited (0) 6 minutes ago           gifted_nobel
```

The container is stopped. We can start it again:

```bash
docker start daemon
```

Let's ensure that it's running:

```bash
docker ps -a
```

Console output:

```
CONTAINER ID  IMAGE   COMMAND                 CREATED        STATUS                    PORTS  NAMES
1fc8cee64ec2  ubuntu  "/bin/sh -c 'while..."  5 minutes ago  Up 3 seconds                     daemon
c006f1a02edf  ubuntu  "/bin/echo 'Hello ..."  6 minutes ago  Exited (0) 7 minutes ago         gifted_nobel
```

Now, stop it again and remove all the containers manually:

```
docker stop daemon
docker rm <your first container name>
docker rm daemon
```

To remove all containers, we can use the following command:

```bash
docker rm -f $(docker ps -aq)
```

* **docker rm** is the command to remove the container.
* **-f** flag (for rm) stops the container if it's running (i.e., force deletion).
* **-q** flag (for ps) is to print only container IDs.
