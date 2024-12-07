

# Deploying the 2048 Game with Docker

This repository demonstrates the process of deploying the classic 2048 game using Docker. The Dockerfile provided automates the setup process, including downloading the game files, configuring the web server (Nginx), and serving the game on port 80.

## Steps to Deploy

### 1. Clone the Repository

Start by cloning the repository:
```bash
git clone https://github.com/yuva19102003/2048.git
cd 2048
```

### 2. Dockerfile Explanation

Below is the content of the `Dockerfile` used to deploy the 2048 game:

```Dockerfile
FROM ubuntu:22.04

RUN apt-get update
RUN apt-get install -y nginx zip curl

RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# Download the zip file
RUN curl -o /var/www/html/master.zip -L https://github.com/yuva19102003/2048/archive/refs/heads/master.zip

# List contents to check the zip file
RUN ls -l /var/www/html/

# Unzip the file
RUN cd /var/www/html/ && unzip master.zip

# List contents after unzip to inspect the extracted files
RUN ls -l /var/www/html/

# Move the files and clean up
RUN cd /var/www/html/ && mv 2048-*/* . && rm -rf 2048-* master.zip

EXPOSE 80
CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]
```

### 3. Build the Docker Image

Build the Docker image using the following command:
```bash
docker build -t 2048-game .
```

### 4. Run the Docker Container

Run the container and map port 80 of the container to port 80 of your host machine:
```bash
docker run -d -p 80:80 2048-game
```

### 5. Access the Game

Open a web browser and navigate to `http://localhost` to play the 2048 game.

## Docker Workflow Summary

1. **Base Image:** The Dockerfile uses `ubuntu:22.04` as the base image.
2. **Dependencies:** Installs Nginx, zip, and curl.
3. **Game Files:** Downloads and unzips the 2048 game files from the GitHub repository.
4. **Web Server Configuration:** Configures Nginx to serve the game files.
5. **Port Exposure:** Exposes port 80 to host the game.
6. **Run Command:** Specifies the command to run Nginx in the foreground.

## Example Commands

- Check running containers:
  ```bash
  docker ps
  ```

- Stop the container:
  ```bash
  docker stop <container_id>
  ```

- Remove the container:
  ```bash
  docker rm <container_id>
  ```

## Troubleshooting

- **Permission Issues:** Ensure Docker is installed and running with proper permissions.
- **Port Conflicts:** If port 80 is in use, map to a different port (e.g., `-p 8080:80`).
- **Access Issues:** Ensure the container is running and accessible by checking the container logs:
  ```bash
  docker logs <container_id>
  ```

## References

- [2048 GitHub Repository](https://github.com/yuva19102003/2048)
- [Docker Documentation](https://docs.docker.com/)

