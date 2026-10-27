\# Docker Networking Troubleshooting



\## Problem Summary



During the Docker networking lab, the `isolated-client` container could not reach the Nginx container using the hostname `network-web`.



The failure occurred because the client was initially connected to Docker's default bridge network instead of the user-defined network used by `network-web`.



\## Environment



\* Platform: Docker Desktop on Windows

\* Network: `docker-learning-network`

\* Network driver: Bridge

\* Web container: `network-web`

\* Client container: `isolated-client`

\* Web server: Nginx



\## 1. Reproduce the Problem



The initial test was:



```cmd

docker exec isolated-client wget -T 5 -O- http://network-web

```



The output was:



```text

wget: bad address 'network-web'

```



\## 2. Interpret the Error



The error indicates that the client could not resolve the hostname `network-web`.



The client was not initially connected to the same user-defined bridge network as the Nginx container.



On a user-defined bridge network, Docker provides automatic name resolution between connected containers. The default bridge network does not provide the same automatic container-name resolution behavior.



\## 3. Apply the Fix



Connected the existing client container to the user-defined network:



```cmd

docker network connect docker-learning-network isolated-client

```



This changed the client's network connections without requiring the container to be recreated.



\## 4. Verify the Fix



Repeated the HTTP request:



```cmd

docker exec isolated-client wget -T 5 -O- http://network-web

```



The request succeeded.



The output included:



```text

Connecting to network-web (172.19.0.2:80)

```



The response contained:



```html

<h1>Welcome to nginx!</h1>

```



\## 5. Verify the Network Configuration



Ran:



```cmd

docker network inspect docker-learning-network

```



The output showed these connected containers:



\* `network-web`

\* `network-client`

\* `isolated-client`



The reported IP addresses were:



| Container         | IP address   |

| ----------------- | ------------ |

| `network-web`     | `172.19.0.2` |

| `network-client`  | `172.19.0.3` |

| `isolated-client` | `172.19.0.4` |



These are the addresses observed during this lab. Docker may assign different addresses if containers or networks are recreated.



\## 6. Root Cause



The client was initially connected to a network that did not provide the required automatic container-name resolution.



\## 7. Resolution



Connected the client to `docker-learning-network` and repeated the request successfully.



\## 8. Lessons Learned



1\. DNS resolution and application connectivity should be investigated separately.

2\. Containers on the same user-defined bridge network can communicate using container names.

3\. Publishing ports to the host is not required for container-to-container communication on a shared network.

4\. `docker network inspect` helps identify network membership and assigned IP addresses.

5\. `docker network connect` can attach an existing container to an additional network.

6\. Reproducing an error, identifying the cause, applying a fix, and verifying the result is a useful troubleshooting workflow.



\## Troubleshooting Workflow



When one container cannot reach another:



1\. Check whether both containers are running using `docker ps`.

2\. Inspect their network configuration using `docker inspect CONTAINER`.

3\. Inspect the relevant network using `docker network inspect NETWORK`.

4\. Test name resolution from the client container.

5\. Test the application port and protocol.

6\. Identify the cause before changing configuration.

7\. Apply the smallest appropriate fix.

8\. Repeat the original test and document the result.



