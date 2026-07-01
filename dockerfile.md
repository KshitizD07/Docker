
DOCKERFILE: It is a text doc that contains all the commands that are required to or could be used on CLI to build the image. This file does all that automatically.
Ex:
FROM Ubuntu
MAINTAINER: KSHITIZ
RUN: apt-get update
CMD: ['npm', 'run', 'dev']

DOCKER BUILD: It builds the image by reading the instructions from the dockerfile.


1:FROM-> It sets the base image for the docker file.
FROM <image>[:TAG] ; FROM Ubuntu:latest

### Docker Image Layers
**What are they?**
Docker images are built from a series of layers. Each instruction in a `Dockerfile` (such as `FROM`, `RUN`, `COPY`) creates a new layer on top of the previous ones. These read-only layers are stacked and represented as a single unified filesystem using a Union File System (UnionFS).

**Example:**
```dockerfile
FROM ubuntu:20.04        # Layer 1: Base Ubuntu OS image
RUN apt-get update       # Layer 2: Adds updated package lists
RUN apt-get install git  # Layer 3: Adds the git binary
COPY . /app              # Layer 4: Adds your application code
```

**Why use layers?**
1. **Caching & Build Speed**: If you change only your application code (`COPY . /app`), Docker doesn't rebuild the OS or reinstall `git`. It simply reuses the cached Layers 1-3 and only rebuilds Layer 4. This drastically speeds up subsequent build times.
2. **Storage Efficiency**: If multiple images share the same base layers (e.g., both use `ubuntu:20.04`), Docker only stores those base layers once on your host's disk, saving massive amounts of storage space.

2:RUN-> It runs one or more commands in new image layer and commits the result automatically.
RUN <commands> ; RUN apt-get update RUN apt-get install -y vim = RUN apt-get update && RUN apt-get install -y \vim \git 

3:CMD-> It is used to specify the command to be executed when the container starts. There should be only one CMD inwhole docker file, else the latest one will take effect.
CMD ['executable', 'param1', 'param2'] 

**4: DOCKER BUILD**
`docker build` is the command used to construct a Docker image from a `Dockerfile` and a "context" (a set of files located in the specified PATH or URL).

**Syntax:**
```bash
docker build [OPTIONS] PATH
```

**Example:**
```bash
# Build an image named "my-node-app" using the current directory (.) as context
docker build -t my-node-app .

# Build an image with a specific tag (version)
docker build -t my-node-app:v1.2 .
```

5:EXPOSE-> Informs dockerthat the container listens on the specified port on runtime.
EXPOSE 80 

6:WORKDIR-> Used to define the working directory of docker container at any given time.
WORKDIR /path ; and run everything in this path.

**7: ENV**
The `ENV` instruction sets environment variables that will be available inside the container at runtime. This is crucial for configuring applications dynamically without hardcoding values in the source code (e.g., database connection strings, API keys, or operational modes).

**Example:**
```dockerfile
# Set a single environment variable
ENV NODE_ENV=production

# Set multiple environment variables
ENV PORT=8080 \
    DB_HOST=localhost
```

8:LABEL-> Used to add the metadata to an image.
LABEL key="value"



BUILD TIME: The phase when docker reads the dockerfile line by line and builds an image from it.
CONTAINER RUNTIME: It is the container execution phase.

Renaming: to add a new name or alias.
docker tag python:10.2.3 p:10.2.3





