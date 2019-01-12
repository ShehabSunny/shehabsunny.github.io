---
layout: post
title:  "Using Docker in Jenkins"
subtitle:
date:   2019-01-12 20:02:01
categories: [productivity, tools]
---

## Introduction
Jenkins offers a simple way to set up a continuous integration or continuous delivery environment for almost any combination of languages and source code repositories using pipelines, as well as automating other routine development tasks. Installing Jenkins is fairly straight forward. With docker, it's even easier.  

However, if we want to create a Jenkins pipeline that uses docker inside of the container then it gets a little painful as the official Jenkins image does not ship with docker pre-installed. There are two different ways to address this problem. One is to use docker-in-docker and another one is to use the host machine's docker inside the Jenkins container. In this post, I will be discussing how can we use the host machine's docker inside of a container, Jenkins in this case.

---
## TL;DR  
If you do not feel like creating the image then you can pull the **pre-build image** using this command `docker pull shehab/jenkins-with-docker` and run using the following command.
```
docker run -d -p 8080:8080 -p 50000:50000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /usr/bin/docker:/usr/bin/docker shehab/jenkins-with-docker 
```
or if you want to use docker-compose use the following `docker-compose.yml` file to define the service and then run `docker-compose up -d` to run the container.
```
version: "3"
services:
    jenkins:
        volumes:
            - '/var/run/docker.sock:/var/run/docker.sock'
            - '/usr/bin/docker:/usr/bin/docker'
        ports:
            - '8080:8080'
            - '50000:50000'
        image: 'shehab/jenkins-with-docker'
```

After the container is running we need to grab the admin password to log in for the first time. The easiest way to do so is to check container logs using the following command.
```
docker logs <container-id> 
# Copy container id from the output of the previous command
```

---

#### Now, Let's get back to using the official Jenkins image and modify it so that we can run docker commands inside.

## Get the Jenkins image
First of all, let's pull the Jenkins image using the following command. This will pull the latest LTS version of Jenkins from docker hub. 
```
docker pull Jenkins/Jenkins:lts
```  

## Run the image
In order to run a container, we need to run the following command.
```
docker run -d -p 8080:8080 -p 50000:50000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /usr/bin/docker:/usr/bin/docker Jenkins/Jenkins:lts 
```

This will print the **container id** in the console. Now, check the logs to get the initial password needed to log into Jenkins web portal. To check the logs of the container we need to run this command.
```
docker logs <container-id> 
# Copy container id from the output of the previous command
```

You will see something like this in the console:
```
*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

875a23d953d2491a9590ef689c0cad2b

This may also be found at: /var/Jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************
```

Grab the password and use it in the web portal for the first login.


## Update the container
At this moment we have docker container of a Jenkins image running on port `8080`. If we visit `localhost:8080` in the browser, we will see the web portal of Jenkins that will ask for the admin password and prompt an installation guide. After finishing the installation process Jenkins can be used to create new pipelines and fully usable. 

However, if we try to create a pipeline that uses `docker` command or try to use `docker` inside the container, it will fail. we will get an error like this:
```
docker: error while loading shared libraries: libltdl.so.7: cannot open shared object file: No such file or directory
```
This is because some libraries are missing. Now, we need to install those missing libraries in the container. In order to do that, we need to get into the container with the following command.
```
docker exec -it -u root <container-id> bash
```
Make sure to change the `<container-id>` and log in as root user with `-u root`.

Now run the following command to install `libltdl7`.
```
apt-get update && apt-get install -y libltdl7 && rm -rf /var/lib/apt/lists/*
```

**At this point we can use docker from the container that is actually connected to the host machine's docker.**

If we run `docker ps -a` then we will be able to see all the docker containers running on the host machine.  

We can now create any Jenkins pipeline that can use docker command to build or deploy using the host machine's docker. **Please note that if we deploy any docker container using the pipeline then it will actually be deployed on the host machine, not inside the Jenkins container.**

## Using docker compose inside Jenkins
If you are planning to use docker compose then you need this step.
The Jenkins image does not come with docker pre-installed, neither the docker-compose. In order to install docker compose we need to run the following commands inside the container just like we installed libltdl7.
```
curl -L https://github.com/docker/compose/releases/download/1.21.3/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
```
chmod +x /usr/local/bin/docker-compose
```

Now, we can use docker-compose inside the Jenkins container as well.

---

Jenkins with Docker can be really handy and make the deployment process a lot easier. Happy deployment.