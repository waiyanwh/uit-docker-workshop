## Example 2: Environment variables and volumes

It's time to create and run more a meaningful container, like **Nginx**.

Change the directory to **Example-2**:
```bash
cd Example-2
```
```bash
docker run -d --name "nginx-demo" -p 8080:80 -v $(pwd)/nginx-demo/html/:/usr/share/nginx/html:ro nginx:latest
```

**Warning:** This command looks quite heavy, but it's just an example to explain volumes and env variables. In 99% of real-life cases, you won't start Docker containers manually –- you'll use customize docker image or use orchestration services (we'll cover [docker-compose](https://docs.docker.com/compose/overview/) in [example 4](../Example-4/README.md)) or write a custom script to do it.

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

We can try to change `$(pwd)/nginx-demo/html` (which is mounted as a volume to `/usr/share/nginx/html` directory inside the container) to `$(pwd)/sample-index` and refresh the page.

Let's get the information about **nginx-demo** container:

```bash
docker inspect nginx-demo
```

This command displays system-wide information about the Docker installation. This information includes the kernel version, number of containers and images, exposed ports, mounted volumes, etc.
