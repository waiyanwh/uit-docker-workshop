# UIT docker workshop

*Reference from* **[alexryabtsev repo](https://github.com/alexryabtsev/docker-workshop)**
![UIT docker workshop](./images/docker.webp)

This is an introductory workshop on Docker containers @ UIT. By the end of this workshop, you will know how to use Docker on your local machine. Along with html,python,php,wordpress, we are going to run Nginx,Redis and mysql containers. Those examples assume that you are familiar with the basic concepts of those technologies.There will be lots of shell examples, so go ahead and open [play-with-docker](https://labs.play-with-docker.com/).

## Table of contents

* [Example 1: hello world](Example-1/README.md)
* [Example 2: Environment variables and volumes](Example-2/README.md)
* [Example 3: Writing your first Dockerfile](Example-3/README.md)
* [Example 4: Connection between containers](Example-4/README.md)
* [Bonus app](Minitwit/README.md)
* [Docker way](#docker-way)
* [Conclusion](#conclusion)

## Docker way

Docker has some restrictions and requirements, depending on the architecture of your system (applications that you pack into containers). You can ignore these requirements or find some workarounds, but in this case, you won't get all the benefits of using Docker. My strong advice is to follow these recommendations:

* **1 application = 1 container**.
* Run process in the **foreground** (don't use systemd, upstart or any other similar tools).
* **Keep data out of container** -- use volumes.
* **Do not use SSH** (if you need to step into container you can use docker exec command).
* **Avoid manual configurations** (or actions) inside container.

## Conclusion

To summarize this workshop, alongside with IDE and Git, Docker has become a must-have developer tool. It's a production-ready tool with a rich and mature infrastructure.

Docker can be used on all types of projects, regardless of size and complexity. In the beginning, you can start with [compose](https://docs.docker.com/compose/overview/) and [Swarm](https://docs.docker.com/engine/swarm/). When the project grows, you can migrate to cloud services like [Amazon Container Services](https://aws.amazon.com/containers/) or [Kubernetes](https://kubernetes.io/).

Like standard containers used in cargo transportation, wrapping your code in Docker containers will help you build faster and more efficient CI/CD processes. This is not just another technological trend promoted by a bunch of geeks –- it's a new paradigm that is already being used in the architecture of large companies like [PayPal](https://blog.docker.com/2017/12/containers-at-paypal/), [Visa](https://blog.docker.com/2017/04/visa-inc-gains-speed-operational-efficiency-docker-enterprise-edition/), [Swisscom](https://www.docker.com/customers/swisscom-goes-400vms-20vms-docker), [General Electric](https://www.docker.com/customers/ge-uses-docker-enable-self-service-their-developers), [Splink](https://www.docker.com/customers/docker-datacenter-delivers-splunks-house-demos), etc.
