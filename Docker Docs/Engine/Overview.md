https://docs.docker.com/storage/

- By default, all files created inside a container are stored on a writable layer meaning
	- It doesn't persist when the container no longer exists and it can be difficult to get data out of the container if another process needs it
	- Writable layer is tightly coupled to the host machine, you can't easily move it elsewhere
	- Writing to a container requires a storage layer to manage the file system

### Choosing the right mount
- No matter which mount type you pick the data looks the same to from within the container
- Different types of mounts:
	- Volumes: stored in part on the host file system which is managed by docker
	- Bind mount: may be stored anywhere on the host system. Non-docker processes can modify them at any time.
	- tmpfs mounts: Stored on the hosts memory only and are never written to the hosts file system

### Volumes
- Stored within a directory on the docker host
- Good for:
	- Sharing data among containers
	- The docker host is not guaranteed to have a file system
	- high IO performance

#### Bind mounts
- Good for:
	- Sharing config files from the host machine
	- sharing source or build artifacts