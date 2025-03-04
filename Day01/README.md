

**Hardware Components:**
- **CPU:** Central Processing Unit, the brain of the computer.
- **RAM:** Random Access Memory, temporary storage for data being used.
- **HD:** Hard Drive, permanent storage for data.
- **Graphic Cards:** Hardware for rendering images and video.

**Software Components:**
- **Operating System:** The main software that manages hardware and software resources.
- **Applications:** Software you use, such as Zen Recorder for recording videos or games.

**Kernel:** The kernel is the core part of an operating system. It acts as a bridge between software and hardware, converting requests into instructions the hardware can understand.

Container Runtimes
There are several runtimes for running containers, including:
Container-D
Docker
CRI-O

# Note: In production environments, Container-D is typically used. Developers often use Docker for local testing.
- **For example,** KIND (Kubernetes IN Docker) is used to create Kubernetes clusters in Docker for testing.


# Container vs Virtual machines 
![Screenshot 2023-02-07 at 7 18 10 PM](https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png)

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/980faa67-603b-46d5-bb0b-83d40a22de08)
---
Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:
  **1.Resource Utilization:**
  - Containers share the host operating system kernel, making them lighter and faster than VMs.
  - VMs have a full-fledged OS and hypervisor, making them more resource-intensive.
    
  **2.Portability:**
  - Containers are designed to be portable and can run on any system with a compatible host operating system.
  - VMs are less portable as they need a compatible hypervisor to run.
    
  **3.Security:**
  - VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs.
  - Containers provide less isolation, as they share the host operating system.
    
  **4.Management:**
  - Managing containers is typically easier than managing VMs, 
  - As containers are designed to be lightweight and fast-moving.


----
### What is Docker ?

Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, Quay.io and so on.

In simple words, you can understand as `containerization is a concept or technology` and `Docker Implements Containerization`.

# Docker Architecture 

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/c3b01248-cf68-49d0-86eb-3aea429b986d)
---
![image](https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png)

#### The above picture, clearly indicates that Docker Deamon is brain of Docker. If Docker Deamon is killed, stops working for some reasons, Docker is brain dead :p (sarcasm intended).
####  Docker follows a client-server architecture, where the Docker Client communicates with the Docker Daemon to manage containers. Here’s the flow:

## Key Components of Docker Architecture:
#### 1.Docker Client:
- The Docker client is used to interact with the Docker daemon (server). Commands like docker build, docker run, and docker pull are sent via the Docker client.
- **Example:** When you type a command like docker run <image-name>, the client sends the request to the Docker Daemon.

#### 2.Docker Daemon (Docker Engine):
- The Docker Daemon is the heart of Docker. It runs in the background and manages Docker containers, images, networks, and volumes.
- The daemon listens for requests from the Docker client and performs the required actions.
- **Example**: When the client requests to start a container, the daemon creates and starts the container.

#### 3.Docker Images:
- A Docker Image is a template that contains the application code and everything needed to run it (libraries, dependencies, etc.).
- It’s read-only and immutable.
- **Example:** You can use an image like ubuntu to run a container with Ubuntu's environment.
  
#### 4.Docker Containers:
- A Docker Container is a running instance of an image. It’s a lightweight, isolated environment where your application runs.
- **Example:** When you run docker run <image-name>, a new container is created from the specified image and started.

#### 5.Docker Registry:
- A Docker Registry is a place where Docker images are stored. Docker Hub is the most common public registry, but you can also set up private registries.
- **Example:** You can pull images from Docker Hub using docker pull <image-name> or push your custom images to a registry using docker push <image-name>.

---
### Docker LifeCycle:
![Screenshot 2023-02-08 at 4 32 13 PM](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png)

The Docker Lifecycle refers to the stages a Docker container goes through from creation to termination. These stages involve building images, running containers, managing containers, and finally, cleaning them up


1. **docker build:** Builds Docker images from a Dockerfile. This image is a snapshot of the application, its dependencies, and environment configuration.
   ```bash
   docker build -t <image-name> .
  ```

2. **docker run:** runs container from docker images (OR) Creates and starts a container from a Docker image. If the image doesn't exist locally, Docker will automatically pull it from the registry.
     ```bash
     docker run -d --name <container-name> <image-name>
     # -d: Runs the container in detached mode (in the background)
    ```

3. **docker push:**  -> push the container image to public/private regestries to share the docker images.
  ```bash
     docker pause <container-name>
 ```

4. **docker ps:** Lists all running containers.
   
5. **docker exec:** Executes a command inside a running container. Commonly used to interact with the container's environment.
    ```bash
         docker exec -it <container-name> <command>
         # -it: Interactive terminal for running commands in the container.
   ```
---

## Why are containers light weight ?

Containers are lightweight because they use a technology called containerization, which allows them to share the host operating system's kernel and libraries, while still providing isolation for the application and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the containers do not need to include a full operating system. Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.

Let's try to understand this with an example:

Below is the screenshot of official ubuntu base image which you can use for your container. It's just ~ 22 MB, isn't it very small ? on a contrary if you look at official ubuntu VM image it will be close to ~ 2.3 GB. So the container base image is almost 100 times less than VM image.

![Screenshot 2023-02-08 at 3 12 38 PM](https://user-images.githubusercontent.com/43399466/217493284-85411ae0-b283-4475-9729-6b082e35fc7d.png)


To provide a better picture of files and folders that containers base images have and files and folders that containers use from host operating system (not 100 percent accurate -> varies from base image to base image). Refer below.



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



### Files and Folders that containers use from host operating system

```
    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
```

It's important to note that while a container uses resources from the host operating system, it is still isolated from the host and other containers, so changes to the container do not affect the host or other containers.

**Note:** There are multiple ways to reduce your VM image size as well, but I am just talking about the default for easy comparision and understanding.

so, in a nutshell, container base images are typically smaller compared to VM images because they are designed to be minimalist and only contain the necessary components for running a specific application or service. VMs, on the other hand, emulate an entire operating system, including all its libraries, utilities, and system files, resulting in a much larger size. 

I hope it is now very clear why containers are light weight in nature.


---
# Installing Docker & Network changes 

curl https://get.docker.com/ | bash 

Netwrok : Bridge - Host - none

Bridge : A Docker bridge network is a private internal network created by Docker on the host machine. It's used to allow containers to communicate with each other within the same host.

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/09f22c6c-743a-480e-8e87-7156d090c65d)
---
---
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
...
---
---
## Listing namespaces & Running basic docker commands

Basic Docker Commands

Check Docker version: docker version

List namespaces: lsns

List PID namespaces: lsns -t pid

Run a container: docker run --name app1 nginx:latest

Run a container in the background: docker -d run --name app1 nginx:latest

Create multiple containers: for i in {1..10}; do docker run -d nginx:latest; done

List running containers: docker ps

Stop all containers: docker stop $(docker ps -aq)

Deploy and remove containers: docker run --rm -d --name app1 nginx:latest

Inspect a container: docker inspect app1

Port forwarding: docker run --rm -d --name app1 -p 8000:80 nginx:latest

View logs: docker logs app1 -f

---



   
