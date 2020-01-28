## Example 2: Environment variables and volumes

It's time to create and run more a meaningful container, like **Nginx**.

Change the directory to **Example-2/**:

```bash
docker run -d --name "nginx-demo" -p 8080:80 -v $(pwd)/nginx-demo/html/:/usr/share/nginx/html:ro nginx:latest
```

**Warning:** This command looks quite heavy, but it's just an example to explain volumes and env variables. In 99% of real-life cases, you won't start Docker containers manually –- you'll use customize docker image or use orchestration services (we'll cover [docker-compose](https://docs.docker.com/compose/overview/) in [example #4](Example-4/README.md) or write a custom script to do it.

Console output:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
683abbb4ea60: Pull complete
a470862432e2: Pull complete
977375e58a31: Pull complete
Digest: sha256:a65beb8c90a08b22a9ff6a219c2f363e16c477b6d610da28fe9cba37c2c3a2ac
Status: Downloaded newer image for nginx:latest
afa095a8b81960241ee92ecb9aa689f78d201cff2469895674cec2c2acdcc61c
```

* **-p** is a ports mapping **HOST PORT:CONTAINER PORT**.
* **-v** is a volume mounting **HOST DIRECTORY:CONTAINER DIRECTORY**.

**Important:** run command accepts only absolute paths. In our example, we've used **$(pwd)** to set the current directory absolute path.

Now check this [url](http://127.0.0.1:8080/) in your web browser.

We can try to change **$(pwd)/nginx-demo/html** (which is mounted as a volume to **/usr/share/nginx/html** directory inside the container) to **$(pwd)/sample-index** and refresh the page.

Let's get the information about **nginx-demo** container:

```bash
docker inspect nginx-demo
```

This command displays system-wide information about the Docker installation. This information includes the kernel version, number of containers and images, exposed ports, mounted volumes, etc.


### Building custom nginx image

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
