# Basic Docker Commands

Here is a comprehensive list of essential Docker commands for managing images and containers.

### 1. `docker pull`
Used to pull (download) an image from a Docker registry (like Docker Hub) to your local machine.
```bash
# Syntax: docker pull [OPTIONS] IMAGE[:TAG]
# ':tag' lets you select a specific version. If not specified, 'latest' is pulled.

# Example: Pulls the Ubuntu 20.04 image
docker pull ubuntu:20.04

# Example: Pulls all available versions of the Ubuntu image
docker pull --all-tags ubuntu  
```

### 2. `docker images`
Lists all the Docker images currently available on your local system.
```bash
docker images
```

### 3. `docker ps`
Lists running containers on the Docker host.
```bash
# Syntax: docker ps [OPTIONS]

# List only running containers
docker ps 

# List all containers (both running and stopped)
docker ps -a 
```

### 4. `docker create`
Creates a new container from an image but leaves it in a **stopped state** (does not start it immediately).
```bash
# Syntax: docker create [OPTIONS] IMAGE [COMMAND] [ARG...]

# Create a container named 'my-container' from the 'nginx' image
docker create --name my-container nginx 

# Create a container with a specific tag
docker create --name my-hello_world hello-world:version
```

### 5. `docker start`
Starts an already created container that is currently stopped.
```bash
# Start a specific container by its name or ID
docker start name-of-container
```

### 6. `docker stop`
Stops a running Docker container gracefully by sending a SIGTERM signal.
```bash
# Stop 'my-container'
docker stop my-container 

# Stop 'my-container' giving it 30 seconds to shut down gracefully before forcing a kill
docker stop -t 30 my-container  
```

### 7. `docker exec`
Executes a command inside an already **running** Docker container. It is incredibly useful for debugging, inspecting the container's file system, processes, and environment variables.
```bash
# Syntax: docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

# Allocates an interactive terminal (-it) to run commands inside the container
docker exec -it my-container ls /app  

# Open a bash shell inside the running container
docker exec -it my-container bash
```

### 8. `docker run`
A combination command that creates and starts a container from a specified image in a single step. (Effectively `docker create` + `docker start`).
```bash
# Syntax: docker run [OPTIONS] IMAGE [COMMANDS][ARG...]

# Create and run a container named 'n1' from 'nginx'
docker run --name n1 nginx

# Run in detached mode (-d), meaning the container runs in the background
docker run --name n1 -d nginx 

# Run the container interactively (-it) and open a shell
docker run -it nginx bash
```

### 9. `docker run --rm`
The `--rm` flag automatically removes the container when it exits. Great for short-lived, disposable tasks.
```bash
# Creates, runs the container, prints the message, and then immediately deletes the container
docker run --rm nginx echo "this will self destruct" 

# Open an interactive shell, and delete the container as soon as you exit the shell
docker run -it --rm nginx bash
```

### 10. `docker inspect`
Retrieves detailed, low-level information about Docker objects (containers, images, volumes, networks) in JSON format. Works for both running and stopped containers.
```bash
# Syntax: docker inspect [OPTIONS] OBJECT[OBJECT...] 

docker inspect my-container
```

### 11. `docker rm`
Removes (deletes) one or more **stopped** containers from your local system.
```bash
# Syntax: docker rm [OPTIONS] CONTAINER [CONTAINER...]

# Remove a specific container
docker rm my-container

# Remove multiple containers at once
docker rm c1 c2 c3

# Force (-f) stop and remove a container even if it is currently running
docker rm -f c1 
```

### 12. `docker rmi`
Removes (deletes) one or more **images** from your local system to free up space.
```bash
# Remove a specific image
docker rmi image-name

# Remove multiple images
docker rmi img1 img2 img3

# Force remove an image even if containers are based on it
docker rmi -f image-name
```

### 13. `docker logs`
Retrieves the output logs of a container. Very useful for troubleshooting and debugging background processes.
```bash
# Print the logs at this specific instance and exit
docker logs c1 

# Follow (-f) the logs and keep the terminal updated with new messages as they stream in
docker logs -f c1 
```

### 14. `docker run --help`
Displays a comprehensive list of available options and flags for the `docker run` command.
```bash
docker run --help
```

### 15. `docker image prune`
Removes all dangling (unused and untagged) images to clean up disk space.
```bash
docker image prune

# To remove all unused images (not just dangling ones)
docker image prune -a
```
