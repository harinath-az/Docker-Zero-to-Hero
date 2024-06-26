# Repo to learn Docker with examples. Contributions are most welcome.

## If you found this repo useful, give it a STAR 🌠

You can watch the video version of this repo on my youtube playlist. -> https://www.youtube.com/watch?v=7JZP345yVjw&list=PLdpzxOOAlwvLjb0vTD9BXLOwwLD_GWCmC


## What is a container ?

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

Ok, let me make it easy !!!

A container is a bundle of Application, Application libraries required to run your application and the minimum system dependencies.

![Screenshot 2023-02-07 at 7 18 10 PM](https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png)

In the diagram shown, the virtual machine on the right side has a complete guest operating system, making it very heavy. In contrast, the container on the left does not have an operating system. It only has a base image or a base operating system, using critical components like the kernel from the host operating system. The container also includes unique elements necessary for creating isolation, which are part of the container's architecture or Docker image.

Containers are lightweight because they do not include a full operating system. Instead, they contain the application, its dependencies, and minimal system dependencies, utilizing the host OS kernel. This logical isolation prevents security breaches. Without logical isolation, a hacker could potentially access all containers in a cluster. Logical isolation is achieved using system dependencies, not the kernel.


## Containers vs Virtual Machine 

Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:

    1. Resource Utilization: Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

    2. Portability: Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.

    3. Security: VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.  
    
    4.  Management: Managing containers is typically easier than managing VMs, as containers are designed to be lightweight and fast-moving.

## Why are containers light weight ?

Containers are lightweight because they use a technology called containerization, which allows them to share the host operating system's kernel and libraries, while still providing isolation for the application and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the containers do not need to include a full operating system. Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.

Let's try to understand this with an example:

Below is the screenshot of official ubuntu base image which you can use for your container. It's just ~ 22 MB, isn't it very small ? on a contrary if you look at official ubuntu VM image it will be close to ~ 2.3 GB. So the container base image is almost 100 times less than VM image.

![Screenshot 2023-02-08 at 3 12 38 PM](https://user-images.githubusercontent.com/43399466/217493284-85411ae0-b283-4475-9729-6b082e35fc7d.png)


To provide a better picture of files and folders that containers base images have and files and folders that containers use from host operating system (not 100 percent accurate -> varies from base image to base image). Refer below.

This image is provided by Ubuntu itself and serves as a container base image. The Ubuntu container base image includes essential files and folders. Any application implemented on this base image will utilize these files and folders from the official Ubuntu Docker image. Meanwhile, other elements such as kernel-related components, networking stack, and file system will be sourced from the host operating system or kernel.

To illustrate, the official Ubuntu Docker image is just 28.16 MB on the Linux AMD64 platform. In comparison, a typical Ubuntu virtual machine image can be as large as 2.3 GB. Although you can reduce the size of both virtual and Docker images, the out-of-the-box difference is significant, with the Docker image being nearly 100 times smaller. This is a major advantage of Docker.

With Docker, instead of running a single virtual machine, you can run 10, 20, or even 30 to 40 containers on the same virtual machine, depending on the resources your containers consume from the host operating system. In cases where containers use substantial resources from the host OS, or if you predefine resource usage for containers (e.g., starting with 256 MB), this scalability might be limited. However, the key advantage of containers is that if they are not running, they allow other containers to utilize resources from the kernel or host OS. This flexibility enables the operation of multiple containers based on their resource usage.

In contrast, virtual machines each have their own guest operating system. The kernel resources used by one virtual machine cannot be shared with another. This isolation prevents virtual machine B from accessing kernel-related resources of virtual machine A, creating a resource usage barrier not present in containers. 



### Files and Folders in containers base images

```
    /bin: contains binary executable files, such as the ls, cp, and ps commands.

    /sbin: contains system binary executable files, such as the init and shutdown commands.

    /etc: contains configuration files for various system services.

    /lib: contains library files that are used by the binary executables.

    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.

    /var: contains variable data, such as log files, spool files, and temporary files.

    /root: is the home directory of the root user.
```
These basic folders form a logical isolation from one container to another container that means a container cannot share these files and folders with another container if a container is
sharing these files and folders that means you are compromising the security of your container or the other container is compromising the security with yourself so that's why these files these files and folders are not used from the kernel and they are basically part of your container image or your container base image.


### Files and Folders that containers use from host operating system

```
    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
```
These are the files we can use from your host operating system and a logical isolation can be created by using this files and folders.

It's important to note that while a container uses resources from the host operating system, it is still isolated from the host and other containers, so changes to the container do not affect the host or other containers.

**Note:** There are multiple ways to reduce your VM image size as well, but I am just talking about the default for easy comparision and understanding.

so, in a nutshell, container base images are typically smaller compared to VM images because they are designed to be minimalist and only contain the necessary components for running a specific application or service. VMs, on the other hand, emulate an entire operating system, including all its libraries, utilities, and system files, resulting in a much larger size. 

I hope it is now very clear why containers are light weight in nature.



## Docker


### What is Docker ?

Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, Quay.io and so on.

In simple words, you can understand as `containerization is a concept or technology` and `Docker Implements Containerization`.


### Docker Architecture ?

![image](https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png)

The above picture, clearly indicates that Docker Deamon is brain of Docker. If Docker Deamon is killed, stops working for some reasons, Docker is brain dead :p (sarcasm intended).

As a client using the Docker client, you can execute commands received by the Docker daemon. The Docker daemon is a process installed when you install Docker; it is essentially the heart of Docker. When you run Docker CLI commands, they are received and executed by the Docker daemon. This daemon creates Docker images and containers and can also push Docker images to your Docker registry. 

The Docker daemon is crucial for Docker's functionality. If the Docker daemon goes down, Docker will stop functioning, and your containers will cease to work because they rely on the daemon to run. 

The architecture involves a client, the Docker CLI, which users interact with to execute Docker commands. These commands are received by the Docker daemon, which then performs the requested actions. For example:
- `docker build` creates a Docker image.
- `docker run` creates a Docker container.
- `docker pull` pulls containers from a registry.

Registries can be private or public, and the Docker daemon knows where to pull or push images based on the account details you provide. To use Docker Hub, you need to create an account, and the Docker daemon will use this information to manage your images.

### Docker LifeCycle 

We can use the below Image as reference to understand the lifecycle of Docker.

There are three important things,

1. docker build -> builds docker images from Dockerfile
2. docker run   -> runs container from docker images
3. docker push  -> push the container image to public/private regestries to share the docker images.

![Screenshot 2023-02-08 at 4 32 13 PM](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png)

Understanding the life cycle of Docker is crucial since the concept of containers is inherently complex. Once you grasp how containers work, using the Docker platform to manage them becomes straightforward. Docker essentially serves as a platform that allows you to create Docker images and containers. You can think of it as a CLI tool or a daemon service that processes your requests.

 ### The flow that happens in docker:

1. **Writing a Dockerfile**:
    - A Dockerfile is a set of instructions for Docker to follow. 
    - For example, you might instruct Docker to use a base image like Ubuntu, add your source code to this image, install necessary libraries and dependencies (e.g., using `npm` for Node.js or `pip` for Python), and set up commands to run your application (e.g., starting a local server).

2. **Building a Docker Image**:
    - Using the `docker build` command, you submit the Dockerfile to the Docker daemon.
    - The Docker daemon then creates a Docker image based on the instructions in the Dockerfile.
    - A Docker image is similar to a snapshot in a virtual machine.

3. **Running a Docker Container**:
    - Once the Docker image is created, you can use the `docker run` command to create a Docker container from this image.
    - The Docker container is the running instance of your image, containing all necessary system dependencies, application libraries, and the application itself.

4. **Sharing and Executing Docker Containers**:
    - Docker containers can be shared with anyone using a public registry.
    - The recipient can download the Docker container and run the application without needing to install any dependencies, as everything is included in the container.

5. **Efficiency and Simplification**:
    - Docker significantly simplifies the deployment process. Instead of manually setting up an environment (e.g., creating an EC2 instance, running the application, exposing ports), Docker automates these steps through the Dockerfile and the Docker daemon.
    - This automation improves efficiency by reducing the number of manual actions required to deploy an application.

6. **End of Lifecycle**:
    - The Docker lifecycle typically ends when the container is no longer needed. You can tear down the container or create new containers as needed.


### Understanding the terminology (Inspired from Docker Docs)


#### Docker daemon

The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.


#### Docker client

The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.


#### Docker Desktop

Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.


#### Docker registries

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.
Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

### Docker Hub
Docker Hub is a public registry that allows you to store and share Docker images with the world. Organizations can also create private repositories within Docker Hub to share images securely within the organization.

### Comparison: Docker Hub vs. GitHub
GitHub: Used for storing and managing source code. It's a version control platform for your code.
Docker Hub: Used for storing Docker images. It's a version control platform for your container images. While GitHub manages your code, Docker Hub manages your application environment encapsulated in Docker images.


#### Dockerfile

Dockerfile is a file where you provide the steps to build your Docker Image. 


#### Images

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

### Drawbacks of using docker
Running the Docker daemon as the root user is considered a drawback because it grants the Docker process full administrative privileges on the host system. This poses a significant security risk since any vulnerability in Docker or within a container could potentially be exploited to gain root access to the host, compromising the entire system. Furthermore, containers initiated by the Docker daemon can inadvertently inherit these root privileges, increasing the potential for security breaches and unintended system modifications. For enhanced security, it is recommended to use rootless Docker or limit the daemon's privileges using user namespaces and other security mechanisms.


1. **Security Risks**: Containers share the host OS kernel, so if the kernel is compromised, all containers can be affected.
   
2. **Complex Networking**: Setting up and managing container networks can be tricky, especially for multi-host setups.

3. **Storage Challenges**: Managing data storage for containers can be difficult, complicating backups and recovery.

4. **Resource Overheads**: Running many containers can strain system resources, even though they are lighter than virtual machines.

5. **Compatibility Issues**: Not all applications are suitable for containers, especially older ones or those needing specific hardware.

6. **Orchestration Complexity**: Using tools like Kubernetes to manage containers at scale adds another layer of complexity.

7. **Performance Overhead**: Containers can slow down performance slightly compared to running applications directly on the host.

8. **Learning Curve**: Docker has a steep learning curve, requiring time to understand and use effectively.

9. **Debugging Challenges**: Debugging within containers can be harder than traditional methods, especially in complex systems.

10. **Configuration Management**: Keeping configurations consistent across different environments (development, staging, production) can be tough and may need additional tools.



## INSTALL DOCKER

A very detailed instructions to install Docker are provide in the below link

https://docs.docker.com/get-docker/

For Demo, 

You can create an Ubuntu EC2 Instance on AWS and run the below commands to install docker.

```
sudo apt update
sudo apt install docker.io -y
```


### Start Docker and Grant Access

A very common mistake that many beginners do is, After they install docker using the sudo access, they miss the step to Start the Docker daemon and grant acess to the user they want to use to interact with docker and run docker commands.

Always ensure the docker daemon is up and running.

A easy way to verify your Docker installation is by running the below command

```
docker run hello-world
```

If the output says:

```
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```

This can mean two things, 
1. Docker deamon is not running.
2. Your user does not have access to run docker commands.


### Start Docker daemon

You use the below command to verify if the docker daemon is actually started and Active

```
sudo systemctl status docker
```

If you notice that the docker daemon is not running, you can start the daemon using the below command

```
sudo systemctl start docker
```


### Grant Access to your user to run docker commands

To grant access to your user to run the docker command, you should add the user to the Docker Linux group. Docker group is create by default when docker is installed.

```
sudo usermod -aG docker ubuntu
```

In the above command `ubuntu` is the name of the user, you can change the username appropriately.

**NOTE:** : You need to logout and login back for the changes to be reflected.


### Docker is Installed, up and running 🥳🥳

Use the same command again, to verify that docker is up and running.

```
docker run hello-world
```

Output should look like:

```
....
....
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
...
```


## Great Job, Now start with the examples folder to write your first Dockerfile and move to the next examples. Happy Learning :)

### Clone this repository and move to example folder

```
git clone https://github.com/iam-veeramalla/Docker-Zero-to-Hero
cd  examples
```

### Login to Docker [Create an account with https://hub.docker.com/]

```
docker login
```

```
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: abhishekf5
Password:
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

### Build your first Docker Image

You need to change the username accoringly in the below command

```
docker build -t abhishekf5/my-first-docker-image:latest .
```
When building a Docker image, you can use a Dockerfile in the current directory without always specifying `-f Dockerfile`. You can simply use `.` to indicate the current directory. Additionally, it's good practice to tag your image using the `-t` flag. This makes it easier to manage and identify your images among potentially hundreds on your machine, laptop, or EC2 instance. Without a tag, your image will be assigned a Docker image ID, which can be difficult to remember. Here's an example command:

```bash
docker build -t my_image:latest .
```

In this command, `-t my_image:latest` tags the image as `my_image` with the tag `latest`. This makes it easier to find and use later.

Output of the above command

```
    Sending build context to Docker daemon  992.8kB
    Step 1/6 : FROM ubuntu:latest
    latest: Pulling from library/ubuntu
    677076032cca: Pull complete
    Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
    Status: Downloaded newer image for ubuntu:latest
     ---> 58db3edaf2be
    Step 2/6 : WORKDIR /app
     ---> Running in 630f5e4db7d3
    Removing intermediate container 630f5e4db7d3
     ---> 6b1d9f654263
    Step 3/6 : COPY . /app
     ---> 984edffabc23
    Step 4/6 : RUN apt-get update && apt-get install -y python3 python3-pip
     ---> Running in a558acdc9b03
    Step 5/6 : ENV NAME World
     ---> Running in 733207001f2e
    Removing intermediate container 733207001f2e
     ---> 94128cf6be21
    Step 6/6 : CMD ["python3", "app.py"]
     ---> Running in 5d60ad3a59ff
    Removing intermediate container 5d60ad3a59ff
     ---> 960d37536dcd
    Successfully built 960d37536dcd
    Successfully tagged abhishekf5/my-first-docker-image:latest
```

### Verify Docker Image is created

```
docker images
```

Output 

```
REPOSITORY                         TAG       IMAGE ID       CREATED          SIZE
abhishekf5/my-first-docker-image   latest    960d37536dcd   26 seconds ago   467MB
ubuntu                             latest    58db3edaf2be   13 days ago      77.8MB
hello-world                        latest    feb5d9fea6a5   16 months ago    13.3kB
```

### Run your First Docker Container

```
docker run -it abhishekf5/my-first-docker-image
```

Output

```
Hello World
```

### Push the Image to DockerHub and share it with the world

```
docker push abhishekf5/my-first-docker-image
```

Output

```
Using default tag: latest
The push refers to repository [docker.io/abhishekf5/my-first-docker-image]
896818320e80: Pushed
b8088c305a52: Pushed
69dd4ccec1a0: Pushed
c5ff2d88f679: Mounted from library/ubuntu
latest: digest: sha256:6e49841ad9e720a7baedcd41f9b666fcd7b583151d0763fe78101bb8221b1d88 size: 1157
```

### You must be feeling like a champ already 
