---
title: "Docker tricks"
categories:
  - Docker
tags:
  - Docker
  - Tricks
---

# **How to Stop All Docker Containers**

To stop all Docker containers, simply run the following command in your terminal:

```docker kill $(docker ps -q)```

## **How It Works**

The `docker ps` command will list all *running* containers. The `-q` flag will only list the IDs for those containers. Once we have the list of all container IDs, we can simply run the `docker kill` command, passing all those IDs, and they’ll all be stopped!

# **How to Remove All Docker Containers**

If you don’t just want to stop containers and you’d like to go a step further and remove them, simply run the following command:

```docker rm $(docker ps -a -q)```

## **How It Works**

We already know that `docker ps -q` will list all running container IDs. What is the `-a` flag? Well that will return *all* containers, not just the running ones. Therefore, this comman will remove all containers (including both running and stopped containers).

# **How To Remove All Docker Images**

To remove all Docker images, run this command:

```docker rmi $(docker images -q)```

## **How It Works**

`docker images -q` will list all image IDs. We pass these IDs to `docker rmi` (which stands for remove images) and we therefore remove all the images.
