# Serving Jupyter Notebook from Cluster

In order to run a jupyter server on a remote container and access it on a local client, we will need a jupyter ready image, configured to allow hosts and a configured ssh tunneling.

## 1. Running Target Container

In order for the server to be discoverable, the flag `--net=host` must be set on container creation, like so:

```bash
docker run --net=host <flags>
```

## 2. Starting Jupyter Notebook Server

Inside the container, the jupyter server must be started with network tags defined, and security disabled.

```bash
jupyter notebook --ip 0.0.0.0 --port 4200 --no-browser --NotebookApp.token=''
```

## 3. SSH Tunneling

With the remote server ready, all that we need now is to set an SSH tunneling between our local and remote hosts.

```bash
ssh -L 4200:hostname:4200 lucasceschini@work
```

There are several variables here specific to a single setup, let's break them down:

- **-L**: This flag is telling ssh command that we are tunnel forwarding.
- **4200**: This is both the local port and remote port defined previously, you can change it to your liking.
- **hostname**: This is the remote server hostname, discoverable due to the passing of the `--net=host` flag under container creation. You must change it to reflect the discoverable machine name that your notebook server is running.
- **lucasceschini**: My remote user.
- **work**: My HostName for the remote server, if not configured, replace it with it's IP address.

## 4. Local Access

Now all that's left is open your favorite jupyter front-end, by visiting `localhost:<port_number>` on a browser, for example.
