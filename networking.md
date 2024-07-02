# Docker Networking

Networking allows containers to communicate with each other and with the host system. Containers run isolated from the host system
and need a way to communicate with each other and with the host system.

By default, Docker provides two network drivers for you, the bridge and the overlay drivers. 

```
docker network ls
```

```
NETWORK ID          NAME                DRIVER
xxxxxxxxxxxx        none                null
xxxxxxxxxxxx        host                host
xxxxxxxxxxxx        bridge              bridge
```


### Bridge Networking

The default network mode in Docker. It creates a private network between the host and containers, allowing
containers to communicate with each other and with the host system.

![image](https://user-images.githubusercontent.com/43399466/217745543-f40e5614-ac34-4b78-85a9-91b24512388d.png)

If you want to secure your containers and isolate them from the default bridge network you can also create your own bridge network.

```
docker network create -d bridge my_bridge
```

Now, if you list the docker networks, you will see a new network.

```
docker network ls

NETWORK ID          NAME                DRIVER
xxxxxxxxxxxx        bridge              bridge
xxxxxxxxxxxx        my_bridge           bridge
xxxxxxxxxxxx        none                null
xxxxxxxxxxxx        host                host
```

This new network can be attached to the containers, when you run these containers.

```
docker run -d --net=my_bridge --name db training/postgres
```

This way, you can run multiple containers on a single host platform where one container is attached to the default network and 
the other is attached to the my_bridge network.

These containers are completely isolated with their private networks and cannot talk to each other.

![image](https://user-images.githubusercontent.com/43399466/217748680-8beefd0a-8181-4752-a098-a905ebed5d2a.png)


However, you can at any point of time, attach the first container to my_bridge network and enable communication

```
docker network connect my_bridge web
```

![image](https://user-images.githubusercontent.com/43399466/217748726-7bb347d0-3736-4f89-bdff-31d240b15150.png)


### Host Networking

This mode allows containers to share the host system's network stack, providing direct access to the host system's network.

To attach a host network to a Docker container, you can use the --network="host" option when running a docker run command. When you use this option, the container has access to the host's network stack, and shares the host's network namespace. This means that the container will use the same IP address and network configuration as the host.

Here's an example of how to run a Docker container with the host network:

```
docker run --network="host" <image_name> <command>
```

Keep in mind that when you use the host network, the container is less isolated from the host system, and has access to all of the host's network resources. This can be a security risk, so use the host network with caution.

Additionally, not all Docker image and command combinations are compatible with the host network, so it's important to check the image documentation or run the image with the --network="bridge" option (the default network mode) first to see if there are any compatibility issues.

### Overlay Networking

This mode enables communication between containers across multiple Docker host machines, allowing containers to be connected to a single network even when they are running on different hosts.

### Macvlan Networking

This mode allows a container to appear on the network as a physical host rather than as a container.




## Additional Explanation for Networking

### Container Communication and Isolation

Before diving into the details, let's outline the two scenarios:
1. **Scenario 1**: Container 1 can communicate with Container 2.
2. **Scenario 2**: Container 1 is isolated from Container 2.

We'll also include a practical demo to solidify these concepts. 

### How Containers Communicate with the Host

To understand container communication, consider how a container interacts with the host operating system. By default, Docker creates a network interface called `eth0` for any resource, including containers and virtual machines. This default network allows containers to communicate with the host.

For example, if your host has an IP address of `192.168.1.1`, Docker creates a virtual Ethernet interface (`docker0`) to bridge the host and containers. This bridge network enables communication despite the different subnets between the host and the container. Without this virtual network, containers wouldn't be able to communicate with the host, rendering them unreachable from the internet and defeating their purpose.

### Default Networking: Bridge Network

The default network in Docker is the bridge network, named `docker0`. This virtual Ethernet allows containers to communicate with the host and other containers. However, all containers share this network, posing security risks since any container can potentially communicate with another container or the host.

### Host Networking

An alternative to the bridge network is host networking, where containers share the host's network directly. This means the container would have an IP address within the host's subnet, enabling seamless communication. However, this method is less secure because it exposes the container to the same vulnerabilities as the host.

### Overlay Networking

Another option is overlay networking, primarily used in multi-host scenarios or container orchestration platforms like Kubernetes. Overlay networks create a unified network across multiple hosts, but they are complex and not necessary for basic Docker usage.

### Custom Bridge Networks

To achieve network isolation while using the bridge network, Docker allows the creation of custom bridge networks. These networks enable logical separation between containers. For instance, a finance container requiring high security can be placed on a custom bridge network, isolating it from other containers using the default bridge network.

#### Practical Demo: Creating and Managing Networks

1. **Run Basic Containers**:
   - Run a basic Nginx container named `login`.
   - Access the container and install necessary utilities (e.g., ping).

   ```sh
   docker run -d --name login nginx
   docker exec -it login /bin/bash
   apt-get update && apt-get install -y iputils-ping
   ```

2. **Run Another Container**:
   - Run a second container named `logout`.
   - Check the IP addresses of both containers and test communication between them.

   ```sh
   docker run -d --name logout nginx
   docker inspect login
   docker inspect logout
   docker exec -it login ping <logout-container-ip>
   ```

3. **Create a Custom Bridge Network**:
   - Create a secure network for a finance container.
   - Attach the finance container to this custom network.

   ```sh
   docker network create secure-network
   docker run -d --name finance --network secure-network nginx
   docker inspect finance
   docker exec -it login ping <finance-container-ip> # This should fail
   ```

4. **Host Networking Example**:
   - Run a container using the host network.
   - Verify its network configuration.

   ```sh
   docker run -d --name host-demo --network host nginx
   docker inspect host-demo
   ```

### Conclusion

Understanding Docker's networking capabilities is essential for managing containerized applications effectively. The bridge network provides a default communication path, while custom bridge networks and host networking offer flexible solutions for different security and connectivity requirements. As we move forward, these concepts will form the foundation for more advanced topics like Kubernetes, which simplifies container orchestration and networking management.

