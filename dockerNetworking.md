
Network Bridges: The physical or virtual device in DLL layer that forwards the traffic between two or more network segments.

### What is a Network Segment?
A **Network Segment** is a portion of a computer network that is separated from the rest of the network by a device such as a bridge, router, or switch. Segments help reduce network congestion and improve security by localizing traffic. In Docker, custom networks act as separate segments, isolating container communication from containers on other networks.

**Docker bridge network**: A software bridge is used in Docker to establish connections between containers running on the same host machine.

When you install and start Docker, a default bridge network called `docker0` is automatically created. Any new container, unless specified otherwise, is attached to this `docker0` network. This allows containers on the same bridge network to communicate with each other via internal IP addresses, while remaining isolated from containers on different networks or the outside world unless explicitly exposed.


Docker Volumes: They provide persistent data storage while using docker. Can be both named and unnamed.
Volumes are stored in host filesystem and is completely managed by docker.
Each volume get its own volume path as- .../docker/volumes/<volume_name>/_data/
It is compeltely isolated from the containers and so the data is persistent and remain independent of the app code. Allows data to survive container restarts, deletion, and image rebuilds.

#### Useful Volume Commands:
```bash
# Create a new named volume
docker volume create <volume_name> 

# List all existing volumes on the host
docker volume ls

# View detailed low-level information about a specific volume
docker volume inspect <volume_name>

# Remove an unused volume (data will be deleted)
docker volume rm <volume_name>
```




