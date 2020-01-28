## Example 3: Writing your first Dockerfile

To build a Docker image, you need to create a Dockerfile. It is a plain text file with instructions and arguments. Here is the description of the instructions we're going to use in our next example:

* **FROM** -- set base image
* **RUN** -- execute command in container
* **ENV** -- set environment variable
* **WORKDIR** -- set working directory
* **VOLUME** -- create mount-point for a volume
* **CMD** -- set executable for container

You can check [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more details.

#### Building custom nginx image

Let us look at how to create a Dockerfile which set up a simple webpage for us.

First, we will build a Docker Image shown below:
```bash
$ cd Example-2/nginx-demo
$ docker build -t nginx-demo .
```
```
Sending build context to Docker daemon 30.21 kB
Step 1/4 : FROM nginx
latest: Pulling from library/nginx
693502eb7dfb: Already exists
6decb850d2bc: Pull complete
c3e19f087ed6: Pull complete
Digest: sha256:52a189e49c0c797cfc5cbfe578c68c225d160fb13a42954144b29af3fe4fe335
Status: Downloaded newer image for nginx:latest
 ---> 6b914bbcb89e
Step 2/4 : COPY wrapper.sh /
 ---> 70297381661d
Removing intermediate container 4e5d9e435617
Step 3/4 : COPY html /usr/share/nginx/html
 ---> 25adcbc67425
Removing intermediate container 8eb8ee64131f
Step 4/4 : CMD ./wrapper.sh
Step 4/4 : CMD ./wrapper.sh
 ---> Running in f4721b3f6421
 ---> c6c187264fad
Removing intermediate container f4721b3f6421
Successfully built c6c187264fad
```

Let us verify if Docker Image is built or NOT:
```bash
$ docker images
```
```
REPOSITORY         TAG                 IMAGE ID            CREATED             SIZE
nginx-demo         latest              c6c187264fad        7 seconds ago       182 MB
```

Running the Container:
```bash
$ docker run -d -P --name myweb nginx-demo
d30904aa94a4615045a2962c3fd15f02bcd82c1b7371f927f77923d60f014645
```

Verifying if Container is running or not:
```bash
$ docker ps
CONTAINER ID        IMAGE        COMMAND             CREATED             STATUS              PORTS                           
                NAMES
d30904aa94a4        nginx-demo   "./wrapper.sh"      3 seconds ago       Up 2 seconds        0.0.0.0:32782->80/tcp, 0.0.0.0:3
2781->443/tcp   myweb
```

**Note:**

Did you encounter this error while cloning and building the Docker Image:

```
docker: Error response from daemon: oci runtime error: container_linux.go:247: starting container process caused "exec: ./wrapper.sh": permission denied".
```

This is due to permission issue. The wrapper.sh script possibly doesn't have executable permission. Run `chmod +x wrapper.sh` and re-build the Docker Image.

Displaying Container Ports:
```bash
$ docker port < container id >
  80/tcp -> 0.0.0.0:32782
  443/tcp -> 0.0.0.0:32781
```

You can open http://localhost:32769 in your browser.

If you want to run Nginx in your desired port, here is the way:

```bash
$ docker run -d -p   8888:80 ajeetraina/nginx-demo-105
beb4fa77b033fb46e9f772110cfcd65e9656af0212931e674a1f2bd34422d478

$ docker ps
CONTAINER ID        IMAGE        COMMAND             CREATED             STATUS              PORTS                           
NAMES
beb4fa77b033        nginx-demo   "./wrapper.sh"      7 seconds ago       Up 5 seconds        443/tcp, 0.0.0.0:8888->80/tcp   
gifted_almeida
```

#### Buliding custom curl image
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
