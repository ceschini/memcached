# Using Docker

## Primary Commands

`docker ps`: check running containers

`docker images`: check local images

stop container:

``` bash
docker rm <CONTAINER_ID>
docker stop <CONTAINER_ID>
```

## Running a Container

```bash
docker run --memory 28G --memory-swap 28G --net=host
--cpus="16"  # number of CPUs
--runtime=nvidia -e NVIDIA_VISIBLE_DEVICES=$gpu_node
-w /my_home  # starting path
# mapped folders
-v ~:/my_home
-v /share_alpha:/share_alpha
-v /share_beta:/share_beta
-u 1192:1197  # user ID and group
-it  # iteractive mode
--name kind_ride  # container name
--rm  # remove container after exit
strink/tensorflow_keras:latest  # image name
/bin/bash  # starting command
```

### Example

Running a tensorflow container with gpu and jupyter support:

```bash
docker run -u $(id -u):$(id -g) -w /my_home -v ~:/my_home -it --rm tensorflow/tensorflow:2.3.0-gpu-jupyter /bin/bash
```

## Networking

To check the host IP address of you container, assuming your container is called `c1`:

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' c1
```

To see what network(s) your container is on, assuming your container is called `c1`:

```
$ docker inspect c1 -f "{{json .NetworkSettings.Networks }}"
```

To disconnect your container from the first network (assuming your first network is called `test-net`):

```
$ docker network disconnect test-net c1
```

Then to reconnect it to another network (assuming it's called test-net-2):

```
$ docker network connect test-net-2 c1
```

To check if two containers (or more) are on a network together:

```
$ docker network inspect test-net -f "{{json .Containers }}"
```

## Some tips and notes

### Image Name Structure

`dockerhub_user/image_name:tag`

Example:

`smitharauco/tensorflowv100cpu:latest`

### Getting User Information

In some cases, running a container without specifying user information through the `-u` flag will deploy and run it as a root user, which is not always preferable.

If you don't know your user info, run the command below or append it to your `docker run` parameters.

```bash
$(id -u):$(id -g)
```

