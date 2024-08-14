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

#### Storage Drivers
- A Docker Docker image is made up of several layers
- Each layer except the last one is a read-only layer
- Commands that modify a filesystem create a layer
- When you create a new container you add a new layer ontop of the underlying ones. Thi slayer is often called the container layer
- All changes made to the running container are written to the writable container layer

![[container-layers.webp]]

- All writes to a container add new or modify existing data stored in the writable layer, the underlying image remains unchanged

#### Copy-on-write strategy
- If a file or directory exists in a lower layer within the image, and another layer (including the writable layer) needs read access to it, it just uses the existing file.
- The first time another layer needs to modify the file (when building or running the container), the file is copied into that layer and modified.