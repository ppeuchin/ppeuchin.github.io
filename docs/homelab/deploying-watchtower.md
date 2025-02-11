# Contents of the file: /[project-name]/[project-name]/homelab/deploying-watchtower.md

# Deploying Watchtower in a Homelab Environment

Watchtower is a useful tool for automatically updating running Docker containers. This guide will walk you through the steps to deploy Watchtower in your homelab environment.

## Prerequisites

- A running Docker environment.
- Basic knowledge of Docker and container management.

## Step 1: Pull the Watchtower Image

First, you need to pull the Watchtower image from Docker Hub. Open your terminal and run the following command:

```bash
docker pull containrrr/watchtower
```

## Step 2: Run Watchtower

To run Watchtower, you can use the following command. This will monitor all running containers and update them when a new image is available.

```bash
docker run -d \
  --name watchtower \
  -e WATCHTOWER_CLEANUP=true \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower
```

### Explanation of the Command:

- `-d`: Runs the container in detached mode.
- `--name watchtower`: Names the container "watchtower".
- `-e WATCHTOWER_CLEANUP=true`: Automatically removes old images after updating.
- `-v /var/run/docker.sock:/var/run/docker.sock`: Allows Watchtower to communicate with the Docker daemon.

## Step 3: Verify Watchtower is Running

You can check if Watchtower is running by executing:

```bash
docker ps
```

You should see the Watchtower container listed among your running containers.

## Conclusion

You have successfully deployed Watchtower in your homelab environment. Watchtower will now keep your Docker containers up to date automatically. For more advanced configurations, refer to the [Watchtower documentation](https://github.com/containrrr/watchtower).