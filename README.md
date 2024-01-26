### Docker Syntax Changes: Adapting to New Standards
In the latest update, Docker has modified its command-line syntax to be more structured and intuitive, shifting from simpler commands like docker build to more explicit ones such as docker image build. This change aims to enhance clarity, precision, and functionality by categorizing commands under specific resource types. While the older syntax remains functional for now, it's crucial for users to adapt to the new syntax to ensure compatibility with future Docker features and updates, necessitating updates in documentation, practice in new projects, and staying informed through Docker's official channels. In this tutorial we will use the nex syntaxes for all the commands.

### Viewing Docker Images
To view Docker images stored on your local machine, you can use the docker images command. This command lists all Docker images that are currently downloaded to your system. It provides useful information such as the repository name, tag, image ID, creation time, and size of each image. This is particularly helpful for managing and organizing your Docker images, and for ensuring you have the necessary images before running containers.  

```bash
docker image ls
```
### Removing Docker Images
To delete Docker images from your local machine, the docker rmi command is used. This command removes one or more images. It's important to note that you can only delete images that are not currently used by any containers. You need to provide either the image ID or the repository name and tag to specify which image(s) you want to remove.

Example Command:
```bash
docker image rm [image ID or repository:tag]
```
For instance, to delete an image with the ID abcdef123456, you would use:
```bash
docker image rm abcdef123456
```
Alternatively, to delete an image using its repository name and tag, such as myimage:latest, you would use:
```bash
docker image rm myimage:latest
```
### Bulk Deleting All Docker Images

To delete all Docker images from your local machine in a single command, you can use a combination of docker image rm and a sub-command to list all image IDs. The command docker image rm $(docker image ls -aq) accomplishes this by first listing all Docker image IDs with docker image ls -aq and then passing these IDs to docker image rm for removal. This method is highly efficient for clearing out all Docker images from your system, including unused and dangling images, thereby freeing up significant disk space.

Example Command:
```bash
docker image rm $(docker image ls -aq)
```
This command executes two operations:

1. `docker image ls -aq`: Lists all Docker image IDs quietly (-q), meaning it only outputs the IDs.
2. `docker image rm`: Removes the Docker images whose IDs are passed from the first command.
3. `-a (or --all)`: This flag is often used with commands that list resources.
4. `-q (or --quiet)`: This flag is used to simplify the output of a command to show only the essential information, usually the IDs.

<!-- It's a powerful command for sweeping clean your Docker image storage, but it should be used with caution as it will remove all your Docker images, irrespective of their status (used or unused). This is particularly useful for developers who need to quickly reset their Docker environment or clear space on their machine. -->

### Listing conatiners:
By default, docker container ls shows only active (running) containers. Adding `-a` includes stopped, exited, or any other non-running containers in the list, providing a comprehensive view of all containers, irrespective of their state.
Example Commands:
List all running containers:
```bash
docker container ls
```
List all containers (running and stopped):
```bash
docker container ls -a
```
List only the IDs of running containers:
```bash
docker container ls -q
```
List the IDs of all containers (running and stopped):
```bash
docker container ls -aq
```
There is another command that does the same as `docker container ls`. The `docker container ps` command, more commonly known as just `docker ps`, is a frequently used Docker command that lists the current containers on your system. This command provides a snapshot of your running containers, including details about their IDs, names, images used, status, ports, and more.

Example Command:
```bash
docker container ps
```

### Stopping conatiners:
The docker container stop command is used to stop a running container.
Example Commands:
List all running containers:
```bash
docker container stop [ contianer ID or name ]
```
```bash
docker container stop my_container
```

To stop all Docker containers on your local machine in a single command, you can use a combination of docker container stop and a sub-command to list all container IDs. 

Example Command:
```bash
docker container stop $(docker container ls -aq)
```
### Removing stopped containers
The `docker container prune` command is used to remove all stopped containers from your local Docker environment. `docker container prune` is particularly useful in scenarios where you have accumulated a large number of stopped containers, such as after testing or development sessions, and you wish to tidy up your environment quickly and efficiently.

Example Command:
```bash
docker container prune
```

### Running a Docker Container
The `docker container run` command is a fundamental Docker command used to create and start a new container from a specified Docker image. This command offers a wide range of options to configure the container's behavior, such as setting environment variables, mapping ports, and defining volume mounts. It's the primary means of initializing containers in the Docker environment.

Example Command:
```bash
docker container run -d -p 80:80 --name mywebserver nginx
```
In this example:

* `-d` runs the container in detached mode, meaning the container runs in the background.<br>
* `-p 80:80` maps port 80 of the container to port 80 on the host, which is essential for web server accessibility.
* `--name mywebserver` assigns a custom name (mywebserver) to the container for easy reference.
* `nginx` is the name of the Docker image used to create the container, in this case, an official Nginx web server image.

This command will start a new container based on the Nginx image, which can be accessed via the host machine's web browser at http://localhost.

### Volume Mounts in Docker
Volume mounts in Docker are a powerful feature used to persist data generated by and used by Docker containers. Unlike data in a container's writable layer, which is tied to the lifecycle of the container, volume mounts are independent and survive container restarts and removals. They allow you to store data on the host system or in a location outside the container, making it possible to access the same data across multiple containers or to keep data persistent when containers are deleted.
There are two primary types of mounts in Docker:
1. **Volumes**: Stored in a part of the host filesystem managed by Docker (/var/lib/docker/volumes/ on Linux). Volumes are completely managed by Docker.
```bash
docker run -d -v NAME:/PATH --name mycontainer httpd:2.4
```
Example Command for Volume Mount:
```bash
docker run -d -v myvolume:/data --name mycontainer httpd:2.4
```
`-v myvolume:/data` creates a volume named myvolume and mounts it to /data inside the container.

2. **Bind Mounts**: Link a specific file or directory on the host to a file or directory in the container. This allows you to persist data but can pose security risks if not managed correctly.

```bash
docker run -d -v /path/on/host:/path/in/container --name mycontainer httpd:2.4
```
`-v /path/on/host:/path/in/container` mounts the host directory /path/on/host to /path/in/container in the container.
This command creates a container where the specified host directory is directly accessible within the container, allowing for data persistence and sharing between the host and the container.
*TIP* The important thing about volume mount is that we need the full working path.
Example Command for Bind Mount:
```bash
docker run -d -v /var/sourcecode:/usr/local/apache2/htdocs/ --name MyWebAPP httpd:2.4
```

### Image Tagging
The `docker tag` command is used to create a tag (alias) for a Docker image, which is especially useful for organizing images into versions, environments, or any other classification system. Tags help in identifying different versions of the same image, such as `latest`, `stable`, `development`, or version numbers like `1.0`, `2.5`.
```bash
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```
Example Command:
```bash
docker tag myapp:latest myapp:stable
```
In this example:

* `myapp:latest` is the source image, where myapp is the image name, and latest is the current tag.
* `myapp:stable` is the target, where myapp remains the image name, and stable is the new tag.

This command will create a new tag for the myapp image. After executing this command, the myapp image will be available under two tags: `latest` and `stable`. Both tags refer to the same image ID but provide different identifiers for use, such as in different environments or deployment stages.

### Executing Commands Inside Docker Containers
The `docker exec` command is used to run specific commands within a running Docker container. This is particularly useful for many purposes such as starting interactive shell sessions, inspecting running processes, modifying files, or interacting with services running inside the container. It provides a way to 'enter' a container to perform tasks or check its state without affecting its current operation.

Example Command:
```bash
docker exec -it my_container /bin/bash
```
In this example:

* `-it` option opens an interactive tty session(terminal).
* `my_container` is the name of the target container.

bash is the command to be executed inside the container, which in this case is starting a bash shell session.
After running this command, you will be placed into a bash shell session inside `my_container`. This is incredibly useful for debugging, monitoring, or modifying the container's environment or file system.
`docker exec` is an essential tool in Docker's toolkit, enabling real-time interaction with containers for a wide range of tasks, from quick modifications to detailed debugging.

### Building Docker Images
The docker image build command is used to create Docker images from a Dockerfile. The Dockerfile provides the instructions for building the image, specifying how to set up and configure the environment inside the container, including copying files, installing software, setting environment variables, and more.

Example Command:
```bash
docker image build -t myapp:1.0 .
```
In this example:

* `-t myapp:1.0` tags the newly created image as myapp with the tag 1.0.
* The `.` specifies that the build context is the current directory (assumed to contain the Dockerfile and any necessary files).

If you want to learn more about Dockerfile click [here!](https://cloudnatives.pro/post/2024/jan/docker-fast-track/)

### Docker Registry and Managing images
Docker Registry is a storage and distribution system for Docker images. It's the backend component of Docker Hub but can also be deployed privately to store, manage, and distribute Docker images within an organization. Docker Registry is essential for controlling the distribution of images, maintaining the lifecycle of Docker applications, and ensuring secure and private storage of proprietary images. Docker Registry can be used for both private, internal use and for public image sharing, similar to Docker Hub but with more control and privacy.
Docker provides two essential commands, `docker pull` and `docker push`, for managing Docker images with remote repositories.

1. **docker pull**:

The `docker pull` command is used to fetch an image from a Docker registry and save it to your local system. This is commonly used to download images from repositories on Docker Hub or other Docker registries.
###

```bash
docker pull registry.example.com/myimage:tag
```
For Docker Hub:
```bash
docker pull ubuntu:20.04
```

2. **docker push**:

Conversely, docker push is used to upload a Docker image from your local system to a remote Docker registry, typically for sharing or backup purposes.
You must tag your local image using your Docker Hub username or the name of another registry first.

  

First, tag the local image with your Docker Hub username:
```bash
docker push myusername/myapp:latest
```
Then push the image:
```bash
docker push myusername/myapp:latest
```
This command pushes the `latest` tag of `myusername/myapp` to Docker Hub.

### docker login
The `docker login` command is used to authenticate to a Docker registry. This is an essential step for pushing and pulling private images or for interacting with any registry that requires authentication. By default, it logs into Docker Hub, but you can specify other registries as well.

Example Command:
- **Logging into Docker Hub**:
```bash
docker login -u myusername -p mypassword
```
- **Logging into a Private Registry**:
```bash
docker login myregistry.example.com -u myusername -p mypassword
```
In this case, `myregistry.example.com` is the address of the private Docker registry. You provide your username (`myusername`) and password (`mypassword`) to authenticate.