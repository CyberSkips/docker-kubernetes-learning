\# Lesson 2: Building Custom Docker Images with Dockerfiles



\## 1. Learning Objectives



In this lesson, I learned how to:



\* Understand the purpose of a Dockerfile.

\* Build a custom Docker image using a Dockerfile.

\* Create and run a container from a custom image.

\* Map a host port to a container port.

\* Verify a running container and test a web application.

\* Understand image tags and versioning.

\* Troubleshoot a container-name conflict.

\* Understand why rebuilding an image does not automatically update an existing container.



\## 2. Project Structure



```text

02-dockerfiles/

├── Dockerfile

├── index.html

└── README.md

```



\## 3. Dockerfile



The Dockerfile used for this exercise:



```dockerfile

FROM nginx:latest



WORKDIR /usr/share/nginx/html



COPY index.html /usr/share/nginx/html/index.html

```



\### Explanation



\* `FROM nginx:latest`: Uses the official Nginx image as the base image.

\* `WORKDIR /usr/share/nginx/html`: Sets the working directory inside the image.

\* `COPY index.html /usr/share/nginx/html/index.html`: Copies the local HTML file into the image so Nginx can serve it.



\## 4. Building the Custom Image



To build version 1.0:



```cmd

docker build -t docker-learning-web:1.0 .

```



To build version 2.0:



```cmd

docker build -t docker-learning-web:2.0 .

```



The `-t` option assigns an image name and tag.



The final `.` specifies the current directory as the Docker build context.



To list the images:



```cmd

docker images docker-learning-web

```



\## 5. Running the Containers



Version 1.0:



```cmd

docker run -d --name docker-learning-web -p 8081:80 docker-learning-web:1.0

```



Version 2.0:



```cmd

docker run -d --name docker-learning-web-v2 -p 8082:80 docker-learning-web:2.0

```



\### Port Mapping



The format is:



```text

\-p HOST\_PORT:CONTAINER\_PORT

```



For example, `-p 8081:80` maps port 8081 on the host computer to port 80 inside the container.



The containers can run simultaneously because they have different names and host ports.



\## 6. Verification



To list running containers:



```cmd

docker ps

```



To test version 1.0:



```cmd

curl http://localhost:8081

```



To test version 2.0:



```cmd

curl http://localhost:8082

```



To inspect a container's status:



```cmd

docker inspect -f "{{.State.Status}}" docker-learning-web

```



To inspect its port mapping:



```cmd

docker port docker-learning-web

```



\## 7. Troubleshooting: Container Name Conflict



\### Problem



When attempting to create a container named `docker-learning-web`, Docker reported that the name was already in use.



\### Cause



A container with that name already existed.



Docker requires container names to be unique, including names belonging to stopped containers.



\### Investigation



I used:



```cmd

docker ps -a

```



This showed running and stopped containers.



I then inspected the existing container:



```cmd

docker inspect docker-learning-web

```



I also checked its status:



```cmd

docker inspect -f "{{.State.Status}}" docker-learning-web

```



And its port mapping:



```cmd

docker port docker-learning-web

```



\### Resolution



The existing container was already running and mapped host port 8081 to container port 80. I used the existing container instead of creating a duplicate.



\## 8. Key Lessons Learned



1\. A Dockerfile contains instructions for building an image.

2\. An image is a packaged template used to create containers.

3\. A container is an instance created from an image.

4\. Docker image tags help identify application versions.

5\. Multiple containers can run the same application image.

6\. Container names must be unique.

7\. Rebuilding an image does not automatically update existing containers.

8\. To apply a rebuilt image, a new container generally needs to be created from it.

9\. Port mapping makes a containerized application accessible through a host port.

10\. Verification commands help confirm whether containers are running and applications are responding.



\## 9. Completion Checklist



\* \[x] Created a Dockerfile.

\* \[x] Built a custom Docker image.

\* \[x] Started a container from the custom image.

\* \[x] Investigated a container-name conflict.

\* \[x] Tested the webpage using curl.

\* \[x] Practised image versioning with versions 1.0 and 2.0.

\* \[x] Verified both application versions.



\## 10. Future Improvements



Possible improvements for future lessons:



\* Use a specific Nginx version instead of `nginx:latest`.

\* Learn about Docker image layers and build-cache optimization.

\* Learn how `.dockerignore` works.

\* Explore Docker networking and persistent storage.

\* Learn how to scan and secure container images.



