# 🛍️ Little Fashion — Dockerized Static Website

A static **Little Fashion** e-commerce website built with HTML, CSS, JavaScript, images, and fonts, and containerized using **Docker + Apache HTTP Server**.

The application is served from an Apache HTTPD container and can be accessed through a browser using a published host port.

> 💡 Every command below is in its own code block — hover over a block on GitHub and click the copy icon in the top-right corner to copy it.

---

## 🚀 Project Overview

This project demonstrates how to:

- Containerize a static HTML/CSS/JavaScript website
- Create a Docker image using an Apache HTTPD base image
- Copy website files into Apache's document root
- Run the application inside a Docker container
- Publish the container's port to the host
- Verify application files and Apache inside the container

---

## 🏗️ Architecture

```
                    Docker Host
                         │
                         │ Port 8000
                         ▼
              ┌─────────────────────┐
              │   Apache Container  │
              │      Port 80        │
              │                     │
              │  /usr/local/apache2 │
              │       /htdocs       │
              └──────────┬──────────┘
                         │
                         ▼
                  Static Website
              HTML + CSS + JS + Images
```

### Request Flow

```
Browser
   │
   │ http://localhost:8000
   ▼
Host Port 8000
   │
   │ Docker Port Mapping
   ▼
Container Port 80
   │
   ▼
Apache HTTP Server
   │
   ▼
/usr/local/apache2/htdocs/index.html
```

---

## 📁 Project Structure

```
Apache-Dockerfile/
│
├── css/
├── fonts/
├── images/
├── js/
│
├── index.html
├── about.html
├── contact.html
├── faq.html
├── product-detail.html
├── products.html
├── sign-in.html
├── sign-up.html
│
├── Dockerfile
├── README.md
└── ABOUT THIS TEMPLATE.txt
```

---

## 🐳 Dockerfile

```dockerfile
FROM httpd:2.4.54-alpine

LABEL maintainer="Sahil Hinge"

COPY . /usr/local/apache2/htdocs/

EXPOSE 80

CMD ["httpd-foreground"]
```

### Dockerfile Explanation

| Instruction | Purpose |
|---|---|
| `FROM` | Uses Apache HTTP Server with Alpine Linux |
| `LABEL` | Adds image metadata |
| `COPY` | Copies website files into Apache's document root |
| `EXPOSE` | Documents that Apache listens on port 80 |
| `CMD` | Starts Apache in the foreground |

### Why `/usr/local/apache2/htdocs/`?

The official Apache HTTPD image uses `/usr/local/apache2/htdocs/` as its default document root. This was verified inside the Apache container before creating the application image.

---

## ⚙️ Prerequisites

Install the following:

- Docker
- Git

Verify Docker:

```bash
docker --version
```

Verify Git:

```bash
git --version
```

---

## 📥 Clone the Repository

Clone the organization repository:

```bash
git clone <YOUR_ORGANIZATION_REPOSITORY_URL>
```

Move into the project:

```bash
cd Docker-projects-master/Apache-Dockerfile
```

Verify the project files:

```bash
ls
```

You should see:

```
Dockerfile
index.html
css
fonts
images
js
```

> Replace `<YOUR_ORGANIZATION_REPOSITORY_URL>` with your actual GitHub organization repository URL.

---

## 🔨 Build the Docker Image

Build the Docker image from the project directory:

```bash
docker build -t apache-website:v1 .
```

**Explanation**

- `docker build` → Builds a Docker image
- `-t apache-website:v1` → Assigns the image name and tag
- `.` → Uses the current directory as the build context

---

## 🔍 Verify the Docker Image

List Docker images:

```bash
docker images
```

You should see:

```
REPOSITORY        TAG
apache-website    v1
```

Inspect the image:

```bash
docker image inspect apache-website:v1
```

---

## ▶️ Run the Docker Container

Create and start the container:

```bash
docker run -d --name apache-web -p 8000:80 apache-website:v1
```

### Port Mapping

```
Host Port 8000  →  Container Port 80
```

The syntax is:

```
-p HOST_PORT:CONTAINER_PORT
```

Therefore `-p 8000:80` means requests to port 8000 on the host are forwarded to Apache's port 80 inside the container.

---

## ✅ Verify the Container

Check running containers:

```bash
docker ps
```

Expected port mapping:

```
0.0.0.0:8000->80/tcp
```

Check all containers (including stopped ones):

```bash
docker ps -a
```

---

## 🌐 Access the Website

If Docker is running on your local machine, open:

```
http://localhost:8000
```

If Docker is running inside a Linux VM, find the VM IP:

```bash
hostname -I
```

Then open (example):

```
http://192.168.56.101:8000
```

---

## 🔎 Verify Website Files Inside the Container

Open a shell inside the running container:

```bash
docker exec -it apache-web sh
```

Check Apache's document root:

```bash
ls /usr/local/apache2/htdocs
```

You should see files such as:

```
index.html
about.html
contact.html
css
fonts
images
js
products.html
```

Exit the container:

```bash
exit
```

---

## 🧪 Test Apache from the Host

Test the website using curl:

```bash
curl http://localhost:8000
```

If the application is working, the HTML content from `index.html` will be returned.

---

## 📋 View Container Logs

Check Apache container logs:

```bash
docker logs apache-web
```

Follow logs in real time:

```bash
docker logs -f apache-web
```

Press `Ctrl + C` to stop following the logs.

---

## 🔄 Container Management

Stop the container:

```bash
docker stop apache-web
```

Start the container:

```bash
docker start apache-web
```

Restart the container:

```bash
docker restart apache-web
```

Remove the container:

```bash
docker rm -f apache-web
```

> If you remove the container, create it again with the `docker run` command from the [Run the Docker Container](#️-run-the-docker-container) section.

---

## 🗑️ Remove the Docker Image

First remove the container if it is using the image:

```bash
docker rm -f apache-web
```

Then remove the image:

```bash
docker rmi apache-website:v1
```

---

## 🏷️ Docker Image Tagging

Create another tag for the same image:

```bash
docker tag apache-website:v1 apache-website:latest
```

Verify:

```bash
docker images
```

---

## 🧹 Recommended `.dockerignore`

Create a `.dockerignore` file:

```bash
touch .dockerignore
```

Add the following contents to it:

```
.git
.gitignore
README.md
.dockerignore
Dockerfile
```

Then rebuild:

```bash
docker build -t apache-website:v2 .
```

### Why use `.dockerignore`?

It prevents unnecessary files from being sent to the Docker build context. This can:

- Reduce build context size
- Improve build performance
- Keep unnecessary files out of the image

> If you want `README.md` or other project documentation inside the container, remove that entry from `.dockerignore`.

---

## 🛠️ Troubleshooting

### Container name already exists

If you get:

```
Conflict. The container name "/apache-web" is already in use
```

Check containers:

```bash
docker ps -a
```

Remove the existing container:

```bash
docker rm -f apache-web
```

Run a new container:

```bash
docker run -d --name apache-web -p 8000:80 apache-website:v1
```

### Website does not open

Check the container:

```bash
docker ps
```

Make sure you have `0.0.0.0:8000->80/tcp` in the output.

Check logs:

```bash
docker logs apache-web
```

Test from the Docker host:

```bash
curl http://localhost:8000
```

Check the Apache document root:

```bash
docker exec apache-web ls /usr/local/apache2/htdocs
```

### Check Port Usage

Check whether port 8000 is already being used:

```bash
sudo ss -lntp | grep :8000
```

If port 8000 is busy, use another host port:

```bash
docker run -d --name apache-web -p 8081:80 apache-website:v1
```

Then access:

```
http://localhost:8081
```

---

## 🔐 Important Docker Concepts Demonstrated

| Concept | Implementation |
|---|---|
| Base Image | `httpd:2.4.54-alpine` |
| Dockerfile | Application image definition |
| Build Context | Project directory `.` |
| Image | `apache-website:v1` |
| Container | `apache-web` |
| Container Port | `80` |
| Host Port | `8000` |
| Port Mapping | `8000:80` |
| Document Root | `/usr/local/apache2/htdocs/` |
| Static Web Server | Apache HTTPD |
| Container Inspection | `docker exec` |
| Logging | `docker logs` |

---

## 👨‍💻 Author

**Sahil Hinge**
DevOps / Cloud Engineering Learner

---

## ⭐ Key Takeaway

```
Source Code
     │
     ▼
Dockerfile
     │
     ▼
Docker Build
     │
     ▼
Docker Image
apache-website:v1
     │
     ▼
Docker Run
     │
     ▼
Container
apache-web
     │
     ▼
Apache :80
     │
     ▼
Host :8000
     │
     ▼
🌐 Website
```

If you're learning Docker for DevOps, this project gives you hands-on practice with the complete flow:

**Dockerfile → Image → Container → Port Mapping → Apache → Website**
