## Example 3: Writing your first Dockerfile

To build a Docker image, you need to create a Dockerfile. It is a plain text file with instructions and arguments. Here is the description of the instructions we're going to use in our next example:

* **FROM** -- set base image
* **RUN** -- execute command in container
* **ENV** -- set environment variable
* **WORKDIR** -- set working directory
* **VOLUME** -- create mount-point for a volume
* **CMD** -- set executable for container

You can check [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more details.

Let's create an image that will get the contents of the website with **curl** and store it to the text file. We need to pass website url via environment variable **SITE_URL**. Resulting file will be placed in a directory mounted as a volume.

Place a file name **Dockerfile** in **examples/curl** directory with the following contents:

```dockerfile
FROM ubuntu:latest
RUN apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y curl \
    && rm -rf /var/lib/apt/lists/*
ENV SITE_URL http://example.com/
WORKDIR /data
VOLUME /data
CMD sh -c "curl -Lk $SITE_URL > /data/results"
```

Dockerfile is ready. It's time to build the actual image.

Go to **examples/curl** directory and execute the following command to build an image:

```bash
docker build . -t test-curl
```

Console output:

```
Sending build context to Docker daemon  3.584kB
Step 1/6 : FROM ubuntu:latest
 ---> 113a43faa138
Step 2/6 : RUN apt-get update     && apt-get install --no-install-recommends --no-install-suggests -y curl     && rm -rf /var/lib/apt/lists/*
 ---> Running in ccc047efe3c7
Get:1 http://archive.ubuntu.com/ubuntu bionic InRelease [242 kB]
Get:2 http://security.ubuntu.com/ubuntu bionic-security InRelease [83.2 kB]
...
Removing intermediate container ccc047efe3c7
 ---> 8d10d8dd4e2d
Step 3/6 : ENV SITE_URL http://example.com/
 ---> Running in 7688364ef33f
Removing intermediate container 7688364ef33f
 ---> c71f04bdf39d
Step 4/6 : WORKDIR /data
Removing intermediate container 96b1b6817779
 ---> 1ee38cca19a5
Step 5/6 : VOLUME /data
 ---> Running in ce2c3f68dbbb
Removing intermediate container ce2c3f68dbbb
 ---> f499e78756be
Step 6/6 : CMD sh -c "curl -Lk $SITE_URL > /data/results"
 ---> Running in 834589c1ac03
Removing intermediate container 834589c1ac03
 ---> 4b79e12b5c1d
Successfully built 4b79e12b5c1d
Successfully tagged test-curl:latest
```

* **docker build** command builds a new image locally.
* **-t** flag sets the name tag to an image.

Now we have the new image, and we can see it in the list of existing images:

```bash
docker images
```

Console output:

```
REPOSITORY  TAG     IMAGE ID      CREATED         SIZE
test-curl   latest  5ebb2a65d771  37 minutes ago  180 MB
nginx       latest  6b914bbcb89e  7 days ago      182 MB
ubuntu      latest  0ef2e08ed3fa  8 days ago      130 MB
```

We can create and run the container from the image. Let's try it with the default parameters:

```bash
docker run --rm -v $(pwd)/vol:/data/:rw test-curl
```

To see results saved to file run:

```bash
cat ./vol/results
```

Let's try with **facebook.com**:

```bash
docker run --rm -e SITE_URL=https://facebook.com/ -v $(pwd)/vol:/data/:rw test-curl
```

To see the results saved to file run:

```bash
cat ./vol/results
```
