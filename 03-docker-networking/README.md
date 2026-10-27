\# Docker Networking



\## Overview



This lesson explores Docker networking, including bridge networks, container-to-container communication, DNS resolution, and network troubleshooting.



The practical exercises use Docker Desktop on Windows and the Nginx and BusyBox container images.



\## Learning Objectives



By completing this lesson, I learned how to:



\* Inspect existing Docker networks.

\* Create a custom bridge network.

\* Connect containers to a user-defined bridge network.

\* Communicate between containers using container names.

\* Understand the difference between container networking and host port publishing.

\* Troubleshoot DNS resolution failures between containers.

\* Connect an existing container to an additional network.

\* Verify network configuration using Docker commands.



\## Key Concepts



\### 1. Docker Networks



Docker networks provide connectivity between containers and, depending on the network configuration, other systems.



Common built-in network types include:



\* `bridge`: Provides networking for containers on a local Docker host.

\* `host`: Uses the host's networking environment, subject to platform-specific behavior.

\* `none`: Provides no normal external container networking.



User-defined bridge networks are useful for connecting related containers.



\### 2. User-Defined Bridge Networks



A user-defined bridge network allows containers to communicate with one another on the same network.



Docker provides automatic container-name resolution on user-defined networks, making it possible to reach another container by name.



\### 3. Container-to-Container Communication



Containers on the same user-defined bridge network can communicate using container names and application ports.



For example:



`http://network-web:80`



The receiving container does not need to publish its port to the Windows host for this internal communication to work.



\### 4. Port Publishing



Port publishing maps a port on the host to a port in a container.



Example:



`-p 8082:80`



This maps host port `8082` to container port `80`.



Port publishing and Docker network selection are separate configuration choices.



\### 5. Docker DNS



Docker provides name resolution for containers connected to the same user-defined network.



This allows a client container to reach another container using its container name instead of relying on a hard-coded IP address.



\## Practical Exercises Completed



\### Exercise 1: Create a Custom Network



Created a user-defined bridge network:



`docker-learning-network`



\### Exercise 2: Start a Web Container



Started an Nginx container named `network-web` on the custom network.



\### Exercise 3: Start a Client Container



Started a BusyBox container named `network-client` on the same network.



\### Exercise 4: Test Container Communication



Used the client container to resolve `network-web` and retrieve the Nginx welcome page.



The request succeeded, confirming DNS resolution and HTTP communication between the containers.



\### Exercise 5: Troubleshoot Network Connectivity



Created an `isolated-client` container on Docker's default bridge network.



The initial request to `http://network-web` failed with:



`wget: bad address 'network-web'`



Connected the existing client to the custom network and repeated the request.



The request then succeeded, returning the Nginx welcome page.



\## Verification



The following commands were used to verify the setup:



\* `docker network ls`

\* `docker network inspect docker-learning-network`

\* `docker ps`

\* `docker exec isolated-client wget -T 5 -O- http://network-web`



The custom network inspection confirmed that the following containers were connected:



\* `network-web`

\* `network-client`

\* `isolated-client`



\## Key Lessons Learned



1\. Containers on the same user-defined bridge network can communicate using container names.

2\. Publishing a port to the host is not required for container-to-container communication on a shared network.

3\. Port publishing and network selection solve different problems.

4\. A container can be connected to an additional network without being recreated.

5\. Docker network inspection helps identify connected containers and their network addresses.

6\. Testing DNS resolution separately from application connectivity helps narrow down networking problems.



\## Environment



\* Operating system: Windows

\* Container platform: Docker Desktop

\* Network driver: Bridge

\* Web server image: `nginx:latest`

\* Client image: `busybox:latest`



\## Completion Status



\* \[x] Inspected Docker networks.

\* \[x] Created a custom bridge network.

\* \[x] Connected containers to the custom network.

\* \[x] Tested container-name resolution.

\* \[x] Verified HTTP communication.

\* \[x] Troubleshot and fixed a network connectivity problem.

\* \[x] Verified the final network configuration.

\* \[ ] Reviewed the documentation and committed the lesson to GitHub.



