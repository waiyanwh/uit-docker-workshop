## Example 4: Connection between containers

[**Docker compose**](https://docs.docker.com/compose/overview/) -- is an CLI utility used to connect containers with each other.

You can install docker-compose [via pip](https://pypi.org/project/docker-compose/) or download binary.

**Pip way:**
```bash
sudo pip install docker-compose
```
**Download binary:**

1. Run this command to download the current stable release of Docker Compose:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
2. Apply executable permissions to the binary:
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

In this example, wa are going to connect Python and Redis containers.

```yaml
version: '3.6'
services:
  app:
    build:
      context: ./app
    depends_on:
      - redis
    environment:
      - REDIS_HOST=redis
    ports:
      - "5000:5000"
  redis:
    image: redis:3.2-alpine
    volumes:
      - redis_data:/data
volumes:
  redis_data:
```

Go to **examples/compose** and execute the following command:

```bash
docker-compose up
```

Console output:

```
Building app
Step 1/9 : FROM python:3.6.3
3.6.3: Pulling from library/python
f49cf87b52c1: Pull complete
7b491c575b06: Pull complete
b313b08bab3b: Pull complete
51d6678c3f0e: Pull complete
09f35bd58db2: Pull complete
1bda3d37eead: Pull complete
9f47966d4de2: Pull complete
9fd775bfe531: Pull complete
Digest: sha256:cdef88d8625cf50ca705b7abfe99e8eb33b889652a9389b017eb46a6d2f1aaf3
Status: Downloaded newer image for python:3.6.3
 ---> a8f7167de312
Step 2/9 : ENV BIND_PORT 5000
 ---> Running in 3b6fe5ca226d
Removing intermediate container 3b6fe5ca226d
 ---> 0b84340fa920
Step 3/9 : ENV REDIS_HOST localhost
 ---> Running in a4f9a1d6f541
Removing intermediate container a4f9a1d6f541
 ---> ebe63bf5959e
Step 4/9 : ENV REDIS_PORT 6379
 ---> Running in fd06aa65fd33
Removing intermediate container fd06aa65fd33
 ---> 2a581c31ff4f
Step 5/9 : COPY ./requirements.txt /requirements.txt
 ---> 671093a12829
Step 6/9 : RUN pip install -r /requirements.txt
 ---> Running in b8ea53bc6ba6
Collecting flask==1.0.2 (from -r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/7f/e7/08578774ed4536d3242b14dacb4696386634607af824ea997202cd0edb4b/Flask-1.0.2-py2.py3-none-any.whl (91kB)
Collecting redis==2.10.6 (from -r /requirements.txt (line 2))
  Downloading https://files.pythonhosted.org/packages/3b/f6/7a76333cf0b9251ecf49efff635015171843d9b977e4ffcf59f9c4428052/redis-2.10.6-py2.py3-none-any.whl (64kB)
Collecting click>=5.1 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/34/c1/8806f99713ddb993c5366c362b2f908f18269f8d792aff1abfd700775a77/click-6.7-py2.py3-none-any.whl (71kB)
Collecting Jinja2>=2.10 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/7f/ff/ae64bacdfc95f27a016a7bed8e8686763ba4d277a78ca76f32659220a731/Jinja2-2.10-py2.py3-none-any.whl (126kB)
Collecting itsdangerous>=0.24 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/dc/b4/a60bcdba945c00f6d608d8975131ab3f25b22f2bcfe1dab221165194b2d4/itsdangerous-0.24.tar.gz (46kB)
Collecting Werkzeug>=0.14 (from flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/20/c4/12e3e56473e52375aa29c4764e70d1b8f3efa6682bef8d0aae04fe335243/Werkzeug-0.14.1-py2.py3-none-any.whl (322kB)
Collecting MarkupSafe>=0.23 (from Jinja2>=2.10->flask==1.0.2->-r /requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/4d/de/32d741db316d8fdb7680822dd37001ef7a448255de9699ab4bfcbdf4172b/MarkupSafe-1.0.tar.gz## MiniTwit an example application written in Python/Flask
Building wheels for collected packages: itsdangerous, MarkupSafe
  Running setup.py bdist_wheel for itsdangerous: started
  Running setup.py bdist_wheel for itsdangerous: finished with status 'done'
  Stored in directory: /root/.cache/pip/wheels/2c/4a/61/5599631c1554768c6290b08c02c72d7317910374ca602ff1e5
  Running setup.py bdist_wheel for MarkupSafe: started
  Running setup.py bdist_wheel for MarkupSafe: finished with status 'done'
  Stored in directory: /root/.cache/pip/wheels/33/56/20/ebe49a5c612fffe1c5a632146b16596f9e64676768661e4e46
Successfully built itsdangerous MarkupSafe
Installing collected packages: click, MarkupSafe, Jinja2, itsdangerous, Werkzeug, flask, redis
Successfully installed Jinja2-2.10 MarkupSafe-1.0 Werkzeug-0.14.1 click-6.7 flask-1.0.2 itsdangerous-0.24 redis-2.10.6
You are using pip version 9.0.1, however version 10.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
Removing intermediate container b8ea53bc6ba6
 ---> 3117d3927951
Step 7/9 : COPY ./app.py /app.py
 ---> 84a82fa91773
Step 8/9 : EXPOSE $BIND_PORT
 ---> Running in 8e259617b7b5
Removing intermediate container 8e259617b7b5
 ---> 55f447f498dd
Step 9/9 : CMD [ "python", "/app.py" ]
 ---> Running in 2ade293ecb25
Removing intermediate container 2ade293ecb25
 ---> b85b4246e9f8

Successfully built b85b4246e9f8
Successfully tagged compose_app:latest
WARNING: Image for service app was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating compose_redis_1 ... done
Creating compose_app_1   ... done
Attaching to compose_redis_1, compose_app_1
redis_1  | 1:C 08 Jul 18:12:21.851 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis_1  |                 _._
redis_1  |            _.-``__ ''-._
redis_1  |       _.-``    `.  `_.  ''-._           Redis 3.2.12 (00000000/0) 64 bit
redis_1  |   .-`` .-```.  ```\/    _.,_ ''-._
redis_1  |  (    '      ,       .-`  | `,    )     Running in standalone mode
redis_1  |  |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
redis_1  |  |    `-._   `._    /     _.-'    |     PID: 1
redis_1  |   `-._    `-._  `-./  _.-'    _.-'
redis_1  |  |`-._`-._    `-.__.-'    _.-'_.-'|
redis_1  |  |    `-._`-._        _.-'_.-'    |           http://redis.io
redis_1  |   `-._    `-._`-.__.-'_.-'    _.-'
redis_1  |  |`-._`-._    `-.__.-'    _.-'_.-'|
redis_1  |  |    `-._`-._        _.-'_.-'    |
redis_1  |   `-._    `-._`-.__.-'_.-'    _.-'
redis_1  |       `-._    `-.__.-'    _.-'
redis_1  |           `-._        _.-'
redis_1  |               `-.__.-'
redis_1  |
redis_1  | 1:M 08 Jul 18:12:21.852 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1  | 1:M 08 Jul 18:12:21.852 # Server started, Redis version 3.2.12
redis_1  | 1:M 08 Jul 18:12:21.852 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
redis_1  | 1:M 08 Jul 18:12:21.852 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
redis_1  | 1:M 08 Jul 18:12:21.852 * The server is now ready to accept connections on port 6379
app_1    |  * Serving Flask app "app" (lazy loading)
app_1    |  * Environment: production
app_1    |    WARNING: Do not use the development server in a production environment.
app_1    |    Use a production WSGI server instead.
app_1    |  * Debug mode: on
app_1    |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
app_1    |  * Restarting with stat
app_1    |  * Debugger is active!
app_1    |  * Debugger PIN: 170-528-240
```

The current example will increment view counter in Redis. Open the [following url](http://127.0.0.1:5000/) in your web browser and check it.

How to use docker-compose is a topic for a separate tutorial. To get started, you can play with some images from Docker Hub. If you want to create your own images, follow the best practices listed above. The only thing I can add in terms of using docker-compose is that you should **always give explicit names to your volumes** in docker-compose.yml ((if the image has volumes). This simple rule will save you from an issue in the future when you'll be inspecting your volumes.

```yaml
version: '3.6'
services:
  ...
  redis:
    image: redis:3.2-alpine
    volumes:
      - redis_data:/data
volumes:
  redis_data:
```

In this case **redis_data** will be the name inside the docker-compose.yml file; for the real volume name, it will be prepended with project name prefix.

To see volumes run:

```bash
docker volume ls
```

Console output:

```
DRIVER              VOLUME NAME
local               apptest_redis_data
```

Without an explicit volume name, there will be UUID. Here's an example from my local machine:

```
DRIVER              VOLUME NAME
local               ec1a5ac0a2106963c2129151b27cb032ea5bb7c4bd6fe94d9dd22d3e72b2a41b
local               f3a664ce353ba24dd43d8f104871594de6024ed847054422bbdd362c5033fc4c
local               f81a397776458e62022610f38a1bfe50dd388628e2badc3d3a2553bb08a5467f
local               f84228acbf9c5c06da7be2197db37f2e3da34b7e8277942b10900f77f78c9e64
local               f9958475a011982b4dc8d8d8209899474ea4ec2c27f68d1a430c94bcc1eb0227
local               ff14e0e20d70aa57e62db0b813db08577703ff1405b2a90ec88f48eb4cdc7c19
local               polls_pg_data
local               polls_public_files
local               polls_redis_data
local               projectdev_pg_data
local               projectdev_redis_data
```
