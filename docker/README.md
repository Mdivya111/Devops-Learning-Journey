# Docker — Complete Notes for AWS DevOps

**Learning Status:** Completed — 10 October 2026  
**Purpose:** AWS DevOps learning, practical revision, and interview preparation

---

# Table of Contents

1. Introduction to Docker
2. Why Docker Is Needed
3. Docker Architecture
4. Docker Images and Containers
5. Docker Installation and Verification
6. Essential Docker Commands
7. Container Lifecycle
8. Dockerfile and Its Instructions
9. Docker Networking
10. Docker Port Mapping
11. Docker Volumes and Persistent Storage
12. Docker Compose
13. Docker Troubleshooting
14. Docker Security Best Practices
15. Important Docker Differences
16. Docker Interview Questions and Answers
17. Quick Command Reference
18. Important Revision Points

---

# 1. Introduction to Docker

## What Is Docker?

Docker is a containerization platform that allows us to package an application and its required dependencies into a standardized unit called a **container image**.

We use that image to create and run containers across different environments.

Docker helps applications run more consistently across development, testing, and production environments.

### Example

Suppose a developer builds an application on their computer, but it fails on another computer because a required library or software version is missing.

Docker helps reduce this problem by packaging the application and its dependencies into an image.

## What Is Containerization?

Containerization is a technology that packages an application and its dependencies into an isolated environment called a container.

Containers share the host operating system's kernel rather than each running a complete guest operating system.

## What Is a Container?

A container is an isolated runtime instance created from an image. It includes the running process and its associated configuration and writable container layer.

Containers can be started, stopped, removed, and recreated.

## What Is a Dependency?

A dependency is software, a library, a package, or another component that an application needs to function correctly.

Examples:

- Python application requiring Python.
- Java application requiring a compatible Java runtime.
- Web application requiring specific libraries.

## Why Is Docker Used?

- Consistent application environments.
- Faster and more repeatable deployments.
- Lightweight application isolation.
- Easier application distribution.
- Efficient use of computing resources.
- Simplified development and testing.
- Integration with CI/CD pipelines and cloud platforms.

## Docker in AWS DevOps

Docker is commonly used with:

- Amazon EC2 to run containers on virtual machines.
- Amazon ECS to orchestrate containers.
- Amazon EKS to run Kubernetes workloads.
- Amazon ECR to store container images.
- Jenkins to build, test, and publish images.
- Terraform to provision supporting infrastructure.

---

# 2. Why Docker Is Needed

Before containerization, teams often faced differences between development, testing, and production environments.

For example:

- The developer uses Python 3.12.
- The testing server uses Python 3.10.
- The application behaves differently because of dependency or runtime differences.

Docker helps package the required application environment so the same image can be used across multiple environments.

### Traditional Deployment vs Containerization

| Traditional deployment | Containerization |
|---|---|
| Dependencies are often installed directly on servers | Dependencies can be packaged into images |
| Environment differences can cause problems | Consistent images reduce environment differences |
| Applications may conflict over dependencies | Containers provide process and filesystem isolation |
| Software setup can be difficult to reproduce | Images provide a repeatable deployment artifact |

**Important:** Containers do not eliminate every environment-related problem. Host architecture, kernel compatibility, external services, configuration, and runtime differences can still matter.

---

# 3. Docker Architecture

Docker uses a client-server architecture.

The main components are:

1. Docker Client
2. Docker Daemon
3. Docker Engine/API
4. Docker Images
5. Docker Containers
6. Docker Networks
7. Docker Volumes
8. Docker Registry

## 3.1 Docker Client

The Docker client is the interface through which users send Docker commands.

Examples:

```bash
docker run nginx
docker pull nginx
docker build -t myapp:1.0 .
docker ps
```

The client sends requests to the Docker daemon through the Docker API.

## 3.2 Docker Daemon

The Docker daemon, `dockerd`, is a background service that manages Docker objects and operations.

It manages:

- Images
- Containers
- Networks
- Volumes

The daemon processes requests sent through the Docker API.

## 3.3 Docker Engine

Docker Engine is the core technology used to build and run containers. It includes the daemon, API, and CLI components, along with supporting functionality.

## 3.4 Docker Image

An image is a read-only template used to create containers.

It can contain:

- Application code
- Libraries
- Dependencies
- Filesystem content
- Image configuration
- Default startup instructions

## 3.5 Docker Container

A container is a runtime instance of an image. It has a writable layer above the image's read-only layers.

## 3.6 Docker Registry

A registry stores and distributes Docker images.

Examples:

- Docker Hub
- Amazon Elastic Container Registry (Amazon ECR)
- Private container registries

## How Docker Works

1. The user enters a Docker command.
2. The Docker client sends the request to the daemon.
3. The daemon performs the requested operation.
4. If the required image is unavailable locally, Docker may pull it from a registry.
5. The daemon creates or manages the container.

### Interview Answer

**What is the Docker daemon?**

The Docker daemon is a background service that receives Docker API requests and manages Docker objects such as images, containers, networks, and volumes.

---

# 4. Docker Images and Containers

## 4.1 Docker Image

A Docker image is a read-only template containing the filesystem and configuration needed to create a container.

Example:

```bash
docker pull nginx
```

This downloads the Nginx image from the configured registry, normally Docker Hub when no other registry is specified.

## 4.2 Docker Container

A container is an instance created from an image.

Example:

```bash
docker run -d --name web1 nginx
```

This creates and starts a container named `web1` using the Nginx image.

## Image vs Container

| Docker Image | Docker Container |
|---|---|
| Template used to create containers | Runtime instance of an image |
| Read-only image layers | Image layers plus a writable container layer |
| Can be used to create many containers | Represents an individual container |
| Built, pulled, tagged, and pushed | Started, stopped, inspected, and removed |

One image can create multiple containers.

## 4.3 Image Tags

A tag identifies a particular named version or variant of an image.

Example:

```bash
docker pull nginx:1.28
```

Here:

- `nginx` is the image repository name.
- `1.28` is the tag.

If a tag is omitted, Docker normally uses `latest`.

**Important:** The `latest` tag does not necessarily mean the newest software version. It is simply a tag that the image publisher can assign to a particular image.

## 4.4 Pulling and Searching Images

```bash
docker pull nginx
docker image ls
docker search nginx
```

- `docker pull` downloads an image.
- `docker image ls` lists local images.
- `docker search` searches Docker Hub for image repositories.

## 4.5 Running a Container

```bash
docker run nginx
```

This creates and starts a container. Without detached mode, the command normally attaches to the container's output.

Run in the background:

```bash
docker run -d --name web1 nginx
```

## 4.6 Interactive Container

```bash
docker run -it --name ubuntu1 ubuntu bash
```

- `-i` keeps standard input open.
- `-t` allocates a pseudo-terminal.
- `--name` assigns a container name.
- `bash` starts the Bash shell when available.

## 4.7 Container Commit

A container's writable-layer changes can be captured in a new image:

```bash
docker commit web1 myapp:snapshot
```

This creates a snapshot-style image from the container.

**Best practice:** Prefer maintaining a Dockerfile and rebuilding an image reproducibly instead of relying on `docker commit` as the normal application build process.

---

# 5. Docker Installation and Verification

After installation, verify Docker:

```bash
docker --version
docker info
docker run hello-world
```

- `docker --version` displays the client version.
- `docker info` displays information about the Docker environment and daemon.
- `docker run hello-world` tests whether Docker can run a simple container.

If Docker cannot connect to the daemon, check whether the daemon or relevant Docker service is running. The exact service command depends on how Docker was installed.

On a system using a systemd-managed Docker Engine:

```bash
sudo systemctl status docker
```

To start the service when appropriate:

```bash
sudo systemctl start docker
```

On Docker Desktop with WSL2, also check Docker Desktop's status and WSL integration settings.

---

# 6. Essential Docker Commands

## 6.1 Image Commands

| Command | Purpose |
|---|---|
| `docker pull nginx` | Download an image |
| `docker image ls` | List local images |
| `docker images` | List local images |
| `docker image inspect nginx` | Inspect image metadata |
| `docker tag myapp:1.0 username/myapp:1.0` | Add a tag |
| `docker push username/myapp:1.0` | Push an image to a registry |
| `docker rmi myapp:1.0` | Remove an image |

## 6.2 Container Commands

| Command | Purpose |
|---|---|
| `docker run nginx` | Create and start a container |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker start web1` | Start a stopped container |
| `docker stop web1` | Stop a running container |
| `docker restart web1` | Restart a container |
| `docker rm web1` | Remove a stopped container |
| `docker rm -f web1` | Force-remove a container |
| `docker logs web1` | View container logs |
| `docker logs -f web1` | Follow logs |
| `docker inspect web1` | Inspect container details |
| `docker exec -it web1 bash` | Open Bash inside a running container, if available |
| `docker stats` | Display live resource usage statistics |
| `docker top web1` | Display processes running in a container |

## 6.3 Container Cleanup

Remove stopped containers:

```bash
docker container prune
```

Remove unused images:

```bash
docker image prune
```

Remove unused Docker objects:

```bash
docker system prune
```

**Warning:** Prune commands delete resources. Review what will be removed before confirming. Do not use broad cleanup commands casually on systems containing important work.

## 6.4 Exec vs Attach

### Exec

```bash
docker exec -it web1 sh
```

Runs a new command inside a running container.

### Attach

```bash
docker attach web1
```

Attaches to the container's existing main process streams.

Use `docker exec` when you want a separate shell or diagnostic command.

**Important:** Detaching from an attached process incorrectly can affect the main process. The default detach key sequence is Ctrl-P followed by Ctrl-Q, but detach keys can be configured.

---

# 7. Container Lifecycle

A typical container lifecycle is:

**Image → Create → Start → Run → Stop → Remove**

Common commands:

```bash
docker create --name web1 nginx
docker start web1
docker stop web1
docker rm web1
```

Alternatively, `docker run` combines creation and starting.

## Running vs Exited

A container can exit when its main process finishes or terminates.

For example:

```bash
docker run hello-world
```

The `hello-world` program prints a message and finishes. Its container normally exits with status `0`.

Check the container:

```bash
docker ps -a
```

Inspect its exit code:

```bash
docker inspect -f '{{.State.ExitCode}}' <container_name>
```

Common exit codes:

- `0`: Process completed successfully.
- `1`: General application error.
- `137`: Process was killed with `SIGKILL`; possible causes include an out-of-memory event or manual termination.

An exit code alone does not always establish the exact cause.

---

# 8. Dockerfile

## 8.1 What Is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

It helps automate and reproduce the image-building process.

Common filename:

```text
Dockerfile
```

Build an image:

```bash
docker build -t myapp:1.0 .
```

Here:

- `docker build` builds an image.
- `-t` assigns the image name and tag.
- `.` specifies the build context as the current directory.

## 8.2 Important Dockerfile Instructions

### FROM

Defines the base image.

```dockerfile
FROM ubuntu:22.04
```

### RUN

Executes commands during image building.

```dockerfile
RUN apt-get update && apt-get install -y git
```

Combining related package operations in one layer and cleaning package metadata when appropriate can reduce image overhead.

### COPY

Copies files from the build context into the image.

```dockerfile
COPY app.txt /app.txt
```

### WORKDIR

Sets the working directory for subsequent instructions and the default working directory for runtime commands.

```dockerfile
WORKDIR /app
```

If the directory does not exist, Docker creates it.

### CMD

Defines the default command or arguments for a container.

```dockerfile
CMD ["echo", "Hello from CMD"]
```

Runtime arguments can replace the default command in the usual exec-form usage.

### ENTRYPOINT

Defines the executable that normally runs when the container starts.

```dockerfile
ENTRYPOINT ["echo"]
```

Arguments supplied after the image name are passed to this executable in exec-form usage.

### CMD and ENTRYPOINT Together

```dockerfile
FROM ubuntu:22.04
ENTRYPOINT ["echo"]
CMD ["Hello"]
```

Running:

```bash
docker run --rm myimage
```

Output:

```text
Hello
```

Running:

```bash
docker run --rm myimage "Welcome Divya"
```

Output:

```text
Welcome Divya
```

**Remember:** In this example, `ENTRYPOINT` defines the executable, while `CMD` provides its default arguments.

### ENV

Sets environment variables in the image configuration.

```dockerfile
ENV APP_ENV=production
```

Environment variables can be read by the application at runtime.

**Security warning:** Do not use `ENV` to bake passwords, API keys, or other secrets into an image.

### EXPOSE

Documents the port on which the application is expected to listen.

```dockerfile
EXPOSE 8080
```

`EXPOSE` does not publish the port to the host by itself. Port publishing is configured separately.

### ADD

Copies files into the image and supports additional behavior, including automatic extraction of certain local tar archives.

```dockerfile
ADD app.tar.gz /app/
```

Use `COPY` for ordinary file copying unless you specifically need `ADD` functionality.

### ARG

Defines a build-time variable.

```dockerfile
ARG APP_VERSION=1.0
RUN echo "Building version ${APP_VERSION}"
```

Override it at build time:

```bash
docker build --build-arg APP_VERSION=2.0 -t myapp:2.0 .
```

**Important:** Do not use build arguments for secrets because build arguments can be exposed through build metadata, history, or other build artifacts.

### USER

Sets the user used by subsequent build instructions and the default runtime process.

```dockerfile
RUN useradd -m appuser
USER appuser
```

Running the application as a non-root user follows the principle of least privilege.

### HEALTHCHECK

Defines a command Docker can use to test whether a container is healthy.

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

This example requires `curl` to be installed and the application to respond successfully at the specified URL.

A health check does not automatically repair a failing application. It reports health status.

## 8.3 .dockerignore

A `.dockerignore` file excludes matching files from the build context.

Example:

```text
.git
.env
node_modules
*.log
```

Benefits:

- Reduces unnecessary build-context data.
- Can improve build efficiency.
- Prevents unnecessary files from being copied into the image.
- Helps reduce accidental inclusion of confidential files.

**Important:** `.dockerignore` is not a complete secrets-management solution. Check what is included in the final image.

## 8.4 Complete Dockerfile Example

```dockerfile
FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY app.txt /app/app.txt

RUN useradd -m appuser

USER appuser

CMD ["cat", "/app/app.txt"]
```

Build and run:

```bash
docker build -t myapp:1.0 .
docker run --rm myapp:1.0
```

The file `app.txt` must exist in the build context. The `cat` command must also be available in the selected base image.

### Additional Instructions for Later Study

The following instructions were postponed for deeper coverage:

- `LABEL`: Adds metadata to an image.
- `SHELL`: Changes the default shell used by shell-form instructions.
- `ONBUILD`: Defines instructions triggered when the image is used as a base image in a subsequent build.

---

# 9. Docker Networking

Docker networking allows containers to communicate with other containers, the host, and external networks according to the network configuration.

## 9.1 Default Bridge Network

Docker creates a default bridge network for standalone containers unless another network mode is specified.

List networks:

```bash
docker network ls
```

Inspect the default bridge:

```bash
docker network inspect bridge
```

Containers on the default bridge can generally communicate using their IP addresses, subject to network configuration. Automatic name resolution between containers is not provided in the same way as on user-defined bridge networks.

## 9.2 User-Defined Bridge Network

Create a network:

```bash
docker network create mynetwork
```

Run a container on it:

```bash
docker run -d --name nginx1 --network mynetwork nginx
```

Start another container on the same network:

```bash
docker run -it --name app1 --network mynetwork ubuntu bash
```

Containers on a user-defined bridge network can normally resolve one another by container name through Docker's embedded DNS, provided the containers are connected to the same network.

The second container still needs a suitable client tool if it is to make HTTP requests.

## 9.3 Network Commands

```bash
docker network ls
docker network create mynetwork
docker network inspect mynetwork
docker network connect mynetwork web1
docker network disconnect mynetwork web1
docker network rm mynetwork
```

## 9.4 Common Network Drivers

| Driver | Purpose |
|---|---|
| `bridge` | Commonly used for standalone containers on one Docker host |
| `host` | Shares the host's network namespace on supported platforms |
| `none` | Provides no ordinary external container networking |
| `overlay` | Supports networking across multiple Docker hosts in suitable configurations |

The availability and behavior of some network modes depend on the platform and deployment configuration.

## 9.5 Docker DNS

Docker DNS allows containers on user-defined networks to resolve other containers by name.

This is more reliable than hardcoding container IP addresses, which can change when containers are recreated.

---

# 10. Docker Port Mapping

Port mapping allows traffic arriving at a host port to be forwarded to a container port.

Example:

```bash
docker run -d --name web1 -p 8080:80 nginx
```

This maps:

- Host port: `8080`
- Container port: `80`

Access the application locally at:

```text
http://localhost:8080
```

Check the published ports:

```bash
docker ps
docker port web1
```

## EXPOSE vs -p

| EXPOSE | `-p` |
|---|---|
| Documents the intended container port | Publishes a container port through the host |
| Declared in the Dockerfile | Used when creating a container or configured through Compose |
| Does not publish the port by itself | Establishes the requested port mapping |

### Port Conflict

An error such as:

```text
port is already allocated
```

means the requested host port is already in use by another container or process.

Troubleshoot:

```bash
docker ps
```

Inspect the `PORTS` column. You may stop the conflicting container if appropriate or choose another host port.

Example:

```bash
docker run -d -p 8081:80 nginx
```

---

# 11. Docker Volumes and Persistent Storage

## 11.1 Why Are Volumes Needed?

A container's writable layer is tied to the container lifecycle. Removing a container removes that writable layer.

Important application data should not rely solely on that layer.

Docker volumes provide storage that exists independently of a container and can be reused by other containers.

## 11.2 Create a Volume

```bash
docker volume create mydata
```

List volumes:

```bash
docker volume ls
```

Inspect a volume:

```bash
docker volume inspect mydata
```

## 11.3 Mount a Volume

```bash
docker run -d \
  --name db1 \
  --mount source=mydata,target=/data \
  ubuntu sleep infinity
```

Create a file inside the mounted volume:

```bash
docker exec db1 sh -c 'echo "Persistent data" > /data/message.txt'
```

Read the file:

```bash
docker exec db1 cat /data/message.txt
```

Remove the container:

```bash
docker rm -f db1
```

Create another container using the same volume:

```bash
docker run --rm \
  --mount source=mydata,target=/data \
  ubuntu cat /data/message.txt
```

Expected output:

```text
Persistent data
```

The file persists because it is stored in the volume, not solely in the first container's writable layer.

## 11.4 Volume Commands

```bash
docker volume create mydata
docker volume ls
docker volume inspect mydata
docker volume rm mydata
docker volume prune
```

A volume generally cannot be removed while it is in use by a container.

**Warning:** `docker volume prune` can delete unused volumes and their data. Check carefully before running it.

## 11.5 Volume vs Bind Mount vs tmpfs

| Storage type | Description |
|---|---|
| Named volume | Managed by Docker and persists independently of containers |
| Bind mount | Maps a specified host file or directory into a container |
| tmpfs | Stores data in memory rather than persistent disk storage |

Use named volumes for many container-persistence use cases. Use bind mounts when direct access to a specific host path is required.

---

# 12. Docker Compose

## 12.1 What Is Docker Compose?

Docker Compose is a tool for defining and managing multi-container applications using a YAML configuration file.

A Compose file can describe:

- Services
- Images or build instructions
- Port mappings
- Environment variables
- Networks
- Volumes
- Service dependencies

Modern Compose files commonly use the filename `compose.yaml` or `docker-compose.yaml`.

Use the Compose V2 command:

```bash
docker compose
```

This differs from the older standalone command `docker-compose`.

## 12.2 Simple Compose Example

Create a file named `compose.yaml`:

```yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

Validate the configuration:

```bash
docker compose config
```

Start the service:

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
```

Test the application:

```bash
curl http://localhost:8080
```

Stop and remove the Compose application:

```bash
docker compose down
```

The top-level `version` field is obsolete in modern Compose and can be omitted.

## 12.3 Multiple Services

```yaml
services:
  web1:
    image: nginx
    ports:
      - "8081:80"

  web2:
    image: nginx
    ports:
      - "8082:80"
```

Start both services:

```bash
docker compose up -d
```

Test each service:

```bash
curl http://localhost:8081
curl http://localhost:8082
```

Both services use Nginx, but each has its own container and host port mapping.

## 12.4 Important Compose Commands

| Command | Purpose |
|---|---|
| `docker compose config` | Validate and render the configuration |
| `docker compose up` | Create and start services |
| `docker compose up -d` | Start services in detached mode |
| `docker compose ps` | Show Compose-managed containers |
| `docker compose logs` | Display service logs |
| `docker compose logs -f` | Follow logs |
| `docker compose stop` | Stop services without removing them |
| `docker compose start` | Start existing stopped services |
| `docker compose down` | Stop and remove the Compose containers and networks it created |
| `docker compose down -v` | Also remove Compose-managed named and anonymous volumes declared for the project |

**Warning:** Do not use `docker compose down -v` unless you intentionally want to remove the relevant volumes and their data. External volumes are not removed by this command.

## 12.5 Why Use Docker Compose?

- Defines multiple services in one configuration.
- Makes local development easier to reproduce.
- Simplifies starting and stopping related containers.
- Provides a convenient way to configure networks and volumes.
- Reduces repetitive manual commands.

**Important:** Docker Compose is useful for defining and managing an application's containers, but it is not a complete replacement for production orchestration systems such as Kubernetes.

---

# 13. Docker Troubleshooting

## Problem 1: Container Does Not Appear in `docker ps`

Use:

```bash
docker ps -a
```

Reason: `docker ps` shows running containers only. `docker ps -a` includes stopped and exited containers.

## Problem 2: Container Exited

Check logs:

```bash
docker logs <container_name>
```

Inspect state and exit code:

```bash
docker inspect <container_name>
docker inspect -f '{{.State.ExitCode}}' <container_name>
```

Logs show application output. Inspect shows detailed state and configuration.

## Problem 3: Cannot Enter a Container Using `docker exec`

If the container is stopped:

```bash
docker start <container_name>
```

Then:

```bash
docker exec -it <container_name> sh
```

Use `bash` instead of `sh` if Bash is available.

If the container immediately exits again, investigate its startup command and logs.

## Problem 4: Port Already Allocated

Check running containers:

```bash
docker ps
```

Inspect the `PORTS` column and use `docker port <container_name>` for an individual container's published mappings.

## Problem 5: Application Is Not Responding

Check:

```bash
docker ps
docker logs <container_name>
docker port <container_name>
docker inspect <container_name>
```

Verify that:

- The container is running.
- The application started successfully.
- The application listens on the expected container port.
- The host port is published correctly.
- Network and firewall rules allow the connection.

## Problem 6: Docker Cannot Connect to the Daemon

Check the daemon or Docker Desktop status, depending on the installation.

For a systemd-managed Docker Engine:

```bash
sudo systemctl status docker
```

If appropriate:

```bash
sudo systemctl start docker
```

The exact fix depends on the operating system and installation method.

## Problem 7: Image Cannot Be Removed

An image may be referenced by a container.

Inspect containers:

```bash
docker ps -a
```

Remove only the relevant containers when it is safe to do so, then retry the image removal.

## Problem 8: Docker Image Build Fails

Check:

- Dockerfile instruction syntax.
- The build context and file paths.
- Image names and tags.
- Package repository connectivity.
- Dependency versions.
- The full build error output.

Build with:

```bash
docker build -t myapp:1.0 .
```

---

# 14. Docker Security Best Practices

## 14.1 Run as a Non-Root User

Use a dedicated, unprivileged application user when possible.

```dockerfile
RUN useradd -m appuser
USER appuser
```

This follows the principle of least privilege and reduces the potential impact of a compromised application.

Container isolation is not an absolute security boundary. Host security and container configuration still matter.

## 14.2 Avoid Unnecessary Packages

Install only what the application needs.

Benefits:

- Smaller images.
- Less unnecessary software.
- Fewer potential vulnerabilities.
- More efficient distribution and deployment.

## 14.3 Do Not Hardcode Secrets

Avoid embedding credentials in Dockerfile instructions such as:

```dockerfile
ENV DB_PASSWORD=mysecret123
```

Instead, use an appropriate secrets-management mechanism for the environment.

For AWS production workloads, AWS Secrets Manager or AWS Systems Manager Parameter Store may be appropriate, depending on the requirements.

A `.env` file is not automatically secure. Protect it and prevent accidental commits.

## 14.4 Use `.dockerignore`

Exclude files that do not belong in the build context.

Example:

```text
.git
.env
*.log
node_modules
```

Review the final image as well; `.dockerignore` alone does not guarantee that secrets are absent.

## 14.5 Scan Images for Vulnerabilities

Use a suitable image scanner to identify known vulnerabilities in operating-system packages and application dependencies.

Review scan results and update vulnerable components as appropriate.

## 14.6 Use Trusted Images

Prefer official or otherwise trusted base images. Pin versions or digests where appropriate to improve reproducibility.

## 14.7 Avoid Publishing Sensitive Information

Before pushing an image to Docker Hub or another registry, verify that it does not contain:

- Passwords or API keys.
- Private SSH keys.
- Confidential configuration.
- Unnecessary development files.
- Sensitive logs.

Inspecting image configuration is useful, but it does not guarantee that every file inside the image is safe.

---

# 15. Important Docker Differences

## 15.1 `docker run` vs `docker start`

| `docker run` | `docker start` |
|---|---|
| Creates a new container and starts it | Starts an existing stopped container |
| Uses an image to create the container | Uses an existing container |

## 15.2 `docker exec` vs `docker attach`

| `docker exec` | `docker attach` |
|---|---|
| Runs a new command in a running container | Connects to the existing main process streams |
| Useful for opening a separate shell | Useful when you need to observe or interact with the main process |

## 15.3 `docker stop` vs `docker rm`

| `docker stop` | `docker rm` |
|---|---|
| Stops a running container | Removes a stopped container |
| Container remains available to start again | Container object is removed |

## 15.4 `docker rmi` vs `docker rm`

| `docker rmi` | `docker rm` |
|---|---|
| Removes an image | Removes a container |

## 15.5 Image vs Volume

An image provides the filesystem template for creating containers. A volume provides storage that can persist independently of the container lifecycle.

## 15.6 `ARG` vs `ENV`

| `ARG` | `ENV` |
|---|---|
| Used for build-time variables | Sets environment variables in image configuration |
| Can be overridden with `--build-arg` | Can be overridden at runtime with supported environment configuration |
| Not suitable for secrets | Not suitable for secrets baked into an image |

## 15.7 `CMD` vs `ENTRYPOINT`

| `CMD` | `ENTRYPOINT` |
|---|---|
| Defines a default command or default arguments | Defines the main executable |
| Runtime arguments commonly replace the default command | Runtime arguments normally become arguments to the executable |
| Often used for defaults | Often used to define the container's main program |

## 15.8 Docker Volume vs Bind Mount

| Docker volume | Bind mount |
|---|---|
| Managed by Docker | Uses a specific host path |
| Suitable for persistent container data | Suitable when direct access to host files is needed |
| Location is managed by Docker | Host path is explicitly specified |

---

# 16. Docker Interview Questions and Answers

## Q1. What is Docker?

Docker is a containerization platform used to package applications and their dependencies into images and run them as isolated containers.

## Q2. What is the difference between an image and a container?

An image is a read-only template used to create containers. A container is a runtime instance of an image with its own writable layer.

## Q3. What is the Docker daemon?

The Docker daemon is a background service that manages Docker objects such as images, containers, networks, and volumes.

## Q4. What is a Dockerfile?

A Dockerfile is a text file containing instructions to build a Docker image.

## Q5. What is Docker Hub?

Docker Hub is a container registry service used to store and distribute images.

## Q6. What is the purpose of `docker pull`?

It downloads an image from a registry to the local Docker environment.

## Q7. What is the difference between `docker ps` and `docker ps -a`?

`docker ps` lists running containers. `docker ps -a` lists all containers, including stopped and exited containers.

## Q8. What is the difference between `docker run` and `docker exec`?

`docker run` creates and starts a container. `docker exec` runs an additional command inside an already running container.

## Q9. Why do containers exit?

A container normally runs while its main process is running. It exits when that process finishes or terminates.

## Q10. How do you investigate a container that has exited?

First, use `docker ps -a` to find it. Then use `docker logs` to review application output and `docker inspect` to examine its state and exit code.

## Q11. What is port mapping?

Port mapping forwards traffic from a host port to a container port. For example, `-p 8080:80` maps host port 8080 to container port 80.

## Q12. What is a Docker volume?

A Docker volume provides persistent storage that exists independently of a container's writable layer.

## Q13. Why do we use a user-defined bridge network?

It allows containers on the same network to communicate and resolve one another by name through Docker's embedded DNS.

## Q14. What is Docker Compose?

Docker Compose defines and manages multi-container applications using a Compose configuration file.

## Q15. What is the difference between `CMD` and `ENTRYPOINT`?

`CMD` defines a default command or arguments. `ENTRYPOINT` defines the executable that normally runs when the container starts.

## Q16. Why should applications run as non-root users?

Running as a non-root user follows least privilege and reduces the potential impact of a security compromise.

## Q17. Why use `.dockerignore`?

It excludes unnecessary files from the build context, helping improve build efficiency and reducing the risk of accidentally including sensitive files.

## Q18. What happens to a volume when a container is removed?

A named volume normally persists after the container is removed. It can be mounted into another container to access the stored data.

## Q19. What causes a port allocation error?

The requested host port is already being used by another process or container. Identify the conflict and use an available port or safely stop the conflicting service.

## Q20. How do you publish an image to Docker Hub?

Tag the image with the correct registry username and repository, authenticate, and push it.

Example:

```bash
docker tag myapp:1.0 username/myapp:1.0
docker login
docker push username/myapp:1.0
```

## Q21. Why should secrets not be placed in Dockerfiles?

Secrets may become exposed through image configuration, layers, build metadata, or other artifacts. Use an appropriate secrets-management mechanism instead.

## Q22. What is the difference between containers and virtual machines?

Containers share the host kernel and provide process isolation. Virtual machines run guest operating systems on virtualized hardware. Containers are often lighter, but both technologies have valid use cases.

## Q23. What is a container registry?

A container registry stores, organizes, and distributes container images. Docker Hub and Amazon ECR are examples.

## Q24. What is the difference between `EXPOSE` and `-p`?

`EXPOSE` documents a container port in the image. `-p` publishes a port through the host.

## Q25. How does Docker help in CI/CD?

Docker provides a repeatable way to package applications into images. CI/CD pipelines can build, test, scan, publish, and deploy those images.

---

# 17. Quick Command Reference

## Images

```bash
docker pull nginx
docker image ls
docker image inspect nginx
docker build -t myapp:1.0 .
docker tag myapp:1.0 username/myapp:1.0
docker push username/myapp:1.0
docker rmi myapp:1.0
```

## Containers

```bash
docker run -d --name web1 nginx
docker run -d -p 8080:80 --name web2 nginx
docker ps
docker ps -a
docker start web1
docker stop web1
docker restart web1
docker logs web1
docker logs -f web1
docker inspect web1
docker exec -it web1 sh
docker stats
docker rm web1
```

## Networks

```bash
docker network ls
docker network create mynetwork
docker network inspect mynetwork
docker network connect mynetwork web1
docker network disconnect mynetwork web1
```

## Volumes

```bash
docker volume create mydata
docker volume ls
docker volume inspect mydata
docker volume rm mydata
```

## Compose

```bash
docker compose config
docker compose up -d
docker compose ps
docker compose logs
docker compose logs -f
docker compose stop
docker compose start
docker compose down
```

## Troubleshooting

```bash
docker ps -a
docker logs <container_name>
docker inspect <container_name>
docker port <container_name>
docker image ls
docker network ls
docker volume ls
docker info
```

---

# 18. Important Revision Points

1. An image is a template; a container is an instance of an image.
2. The Docker client sends requests; the daemon manages Docker operations.
3. `docker run` creates and starts a new container.
4. `docker ps` shows running containers; `docker ps -a` shows all containers.
5. `docker logs` displays container output.
6. `docker inspect` shows detailed configuration and state.
7. `docker exec` runs a command in a running container.
8. `docker stop` stops a container; `docker rm` removes it.
9. `docker rmi` removes an image.
10. A Dockerfile defines how to build an image.
11. `CMD` provides a default command or arguments; `ENTRYPOINT` defines the executable.
12. `EXPOSE` does not publish a host port.
13. A user-defined bridge network supports container-name resolution.
14. Named volumes persist independently of container removal.
15. Docker Compose defines and manages multi-container applications.
16. Non-root execution and minimal images improve security.
17. Secrets must not be hardcoded into images.
18. Logs and inspect output help diagnose container failures.
19. Images should be checked and scanned before publishing.
20. Docker knowledge supports CI/CD, cloud deployment, and container orchestration.

---

## Final Status

**Docker learning: Completed at the fundamentals and job-preparation level**

Topics covered include Docker architecture, image and container management, Dockerfiles, networking, port mapping, volumes, Compose, troubleshooting, security, and interview revision.

**End of Docker Notes**
