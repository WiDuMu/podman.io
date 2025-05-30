---
id: podman
title: Getting Started with Podman
---

![Podman logo](/logos/raw/podman-logo-orig.png#gh-light-mode-only)![Podman logo](/logos/raw/podman-logo-dark.png#gh-dark-mode-only)

# Getting Started with Podman

Podman is a utility provided as part of the libpod library. It can be used to
create and maintain containers. The following tutorial will teach you how to set
up Podman and perform some basic commands.

## Podman Documentation

The documentation for Podman is located
[here](https://docs.podman.io).

## Installing Podman

For installing or building Podman, please see the
[installation instructions](docs/installation).

## Familiarizing yourself with Podman

The code samples are intended to be run as a non-root user, and use
`sudo` where root escalation is required.

### Getting help

To get some help and find out how Podman is working, you can use the _help_:

```bash
podman --help
podman <subcommand> --help
```

For more details, you can review the manpages:

```bash
man podman
man podman-<subcommand>
```

Please also reference the [Podman Troubleshooting Guide](https://github.com/containers/podman/blob/main/troubleshooting.md)
to find known issues and tips on how to solve common configuration mistakes.

### Searching, pulling & listing images

Podman can search for images on remote registries with some simple keywords.

```bash
podman search <search_term>
```

You can also enhance your search with filters:

```bash
podman search httpd --filter=is-official
```

Downloading (Pulling) an image is easy, too.

```bash
podman pull docker.io/library/httpd
```

After pulling some images, you can list all images, present on your machine.

```bash
podman images
```

**Note**: Podman searches in different registries. Therefore it is recommend
to use the full image name (_docker.io/library/httpd_ instead of
_httpd_) to ensure, that you are using the correct image.

### Running a container

This sample container will run a very basic httpd server that serves only its
index page.

```bash
podman run -dt -p 8080:80/tcp docker.io/library/httpd
```

**Note**: Because the container is being run in detached mode, represented by
the `-d` in the `podman run` command, Podman will print the container ID after
it has executed the command. The `-t` also adds a pseudo-tty to run arbitrary
commands in an interactive shell.

**Note**: We use port forwarding to be able to access the HTTP server. For
successful running at least slirp4netns v0.3.0 is needed.

### Listing running containers

The `podman ps` command is used to list created and running containers.

```bash
podman ps
```

**Note**: If you add `-a` to the `podman ps` command, Podman will show all
containers (created, exited, running, etc.).

### Testing the httpd container

As you are able to see, the container does not have an IP Address assigned. The
container is reachable via it's published port on your local machine.

```bash
curl http://localhost:8080
```

From another machine, you need to use the IP Address of the host, running the
container.

```bash
curl http://<IP_Address>:8080
```

**Note**: Instead of using curl, you can also point a browser to
`http://localhost:8080`.

### Inspecting a running container

You can "inspect" a running container for metadata and details about itself.
`podman inspect` will provide lots of useful information like environment
variables, network settings or allocated resources.

Since, the container is running in **rootless** mode, no IP Address is assigned
to the container.

```bash
podman inspect -l | grep IPAddress
            "IPAddress": "",
```

**Note**: The `-l` is a convenience argument for **latest container**. You can
also use the container's ID or name instead of `-l` or the long argument
`--latest`.

**Note**: If you are running remote Podman client, including Mac and Windows
(excluding WSL2) machines, `-l` option is not available.

### Viewing the container's logs

You can view the container's logs with Podman as well:

```bash
podman logs -l

127.0.0.1 - - [04/May/2020:08:33:48 +0000] "GET / HTTP/1.1" 200 45
127.0.0.1 - - [04/May/2020:08:33:50 +0000] "GET / HTTP/1.1" 200 45
127.0.0.1 - - [04/May/2020:08:33:51 +0000] "GET / HTTP/1.1" 200 45
127.0.0.1 - - [04/May/2020:08:33:51 +0000] "GET / HTTP/1.1" 200 45
127.0.0.1 - - [04/May/2020:08:33:52 +0000] "GET / HTTP/1.1" 200 45
127.0.0.1 - - [04/May/2020:08:33:52 +0000] "GET / HTTP/1.1" 200 45
```

### Viewing the container's pids

You can observe the httpd pid in the container with `podman top`.

```bash
podman top -l

USER     PID   PPID   %CPU    ELAPSED            TTY     TIME   COMMAND
root     1     0      0.000   22m13.33281018s    pts/0   0s     httpd -DFOREGROUND
daemon   3     1      0.000   22m13.333132179s   pts/0   0s     httpd -DFOREGROUND
daemon   4     1      0.000   22m13.333276305s   pts/0   0s     httpd -DFOREGROUND
daemon   5     1      0.000   22m13.333818476s   pts/0   0s     httpd -DFOREGROUND
```

### Stopping the container

You may stop the container:

```bash
podman stop -l
```

You can check the status of one or more containers using the `podman ps`
command. In this case, you should use the `-a` argument to list all containers.

```bash
podman ps -a
```

### Removing the container

Finally, you can remove the container:

```bash
podman rm -l
```

You can verify the deletion of the container by running `podman ps -a`.

## Network

For a more detailed guide about Networking and DNS in containers, please see the
[network guide](https://github.com/containers/podman/blob/main/docs/tutorials/basic_networking.md).

## Checkpoint, Migration and Restoring containers

Checkpointing a container stops the container while writing the state of all
processes in the container to disk. With this, a container can later be
migrated and restored, running at exactly the same point in time as the
checkpoint. For more details, see the
[checkpoint instructions](docs/checkpoint).

## Integration Tests

For more information on how to setup and run the integration tests in your
environment, checkout the Integration Tests
[README.md](https://github.com/containers/podman/blob/main/test/README.md).

## Podman Python Documentation

The documentation for the Podman Python SDK is located
[here](https://podman-py.readthedocs.io/en/latest/index.html).

## More information

For more information on Podman and its subcommands, checkout the asciiart demos
on the [README.md](https://github.com/containers/podman/blob/main/commands-demo.md)
page.
