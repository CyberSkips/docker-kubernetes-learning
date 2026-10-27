\# Docker Networking Command Reference



\## 1. List Docker Networks



```cmd

docker network ls

```



\*\*Purpose:\*\* Lists Docker networks available on the host.



\*\*What to check:\*\* Network names, IDs, and drivers.



\---



\## 2. Inspect a Network



```cmd

docker network inspect docker-learning-network

```



\*\*Purpose:\*\* Displays detailed information about a network.



\*\*What to check:\*\*



\* Network driver.

\* Subnet and gateway.

\* Connected containers.

\* Container IP addresses.



\---



\## 3. Create a Custom Bridge Network



```cmd

docker network create --driver bridge docker-learning-network

```



\*\*Purpose:\*\* Creates a user-defined bridge network.



\*\*Note:\*\* This command fails if a network with that name already exists. Check `docker network ls` before creating it.



\---



\## 4. Start an Nginx Container on the Custom Network



```cmd

docker run -d --name network-web --network docker-learning-network nginx:latest

```



\*\*Purpose:\*\* Starts Nginx in detached mode and connects the container to the custom network.



\*\*Important:\*\* This command does not publish a port to the Windows host.



\---



\## 5. Start a BusyBox Client



```cmd

docker run -dit --name network-client --network docker-learning-network busybox:latest sh

```



\*\*Purpose:\*\* Starts a BusyBox container with an interactive shell process, allowing commands to be executed inside it.



\---



\## 6. Check Running Containers



```cmd

docker ps

```



\*\*Purpose:\*\* Lists running containers.



To include stopped containers:



```cmd

docker ps -a

```



\---



\## 7. Test DNS Resolution



```cmd

docker exec -it network-client sh

```



Inside the container, run:



```sh

nslookup network-web

```



\*\*Purpose:\*\* Tests whether the client can resolve the Nginx container's name.



Exit the shell with:



```sh

exit

```



\---



\## 8. Test HTTP Communication



```cmd

docker exec network-client wget -T 5 -O- http://network-web

```



\*\*Purpose:\*\* Sends an HTTP request to Nginx and prints the response.



\*\*Expected result:\*\* HTML containing the Nginx welcome page.



\---



\## 9. Connect an Existing Container to a Network



```cmd

docker network connect docker-learning-network isolated-client

```



\*\*Purpose:\*\* Connects an existing container to an additional network.



\*\*Note:\*\* The container must exist, and it must not already be connected to that network.



\---



\## 10. Disconnect a Container from a Network



```cmd

docker network disconnect docker-learning-network isolated-client

```



\*\*Purpose:\*\* Disconnects a container from the specified network.



\*\*Caution:\*\* Only run this when you intend to change the container's network connectivity.



\---



\## 11. Inspect a Container



```cmd

docker inspect network-web

```



\*\*Purpose:\*\* Displays detailed information about a container, including its network configuration.



\---



\## 12. Publish a Container Port



Example:



```cmd

docker run -d --name published-web -p 8085:80 nginx:latest

```



\*\*Purpose:\*\* Maps host port `8085` to container port `80`.



Open the following address in a browser on the host:



`http://localhost:8085`



\*\*Important:\*\* Choose an available host port and a container name that is not already in use.



\---



\## Command Summary



| Command                                       | Main purpose                                 |

| --------------------------------------------- | -------------------------------------------- |

| `docker network ls`                           | List networks                                |

| `docker network inspect NETWORK`              | Inspect a network                            |

| `docker network create`                       | Create a network                             |

| `docker network connect NETWORK CONTAINER`    | Connect a container                          |

| `docker network disconnect NETWORK CONTAINER` | Disconnect a container                       |

| `docker ps`                                   | List running containers                      |

| `docker ps -a`                                | List all containers                          |

| `docker inspect CONTAINER`                    | Inspect a container                          |

| `docker exec CONTAINER COMMAND`               | Execute a command inside a running container |



\## Safety Reminder



Before running commands that create or change resources, check whether the resource already exists.



Avoid deleting containers or networks simply to resolve a problem. Inspect the current configuration first, identify the cause, and apply the smallest appropriate change.



