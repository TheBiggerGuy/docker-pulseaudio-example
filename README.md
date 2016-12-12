# Docker PulseAudio Example
An example of a PulseAudio app working within a Docker container using the hosts sound system

[Docker Hub](https://hub.docker.com/r/thebiggerguy/docker-pulseaudio-example/)

## Building
```bash
./buid.sh
```

## Running
```bash
./host_runner.sh
```

## Explanation
Getting a PulseAudio app to work within a Docker container is harder than it looks. This example is designed to produce the smallest working configuration that can be used in your own containers.

To be clear this is designed so the app within the Docker container does not run a PulseAudio server. Instead the app connects as a client directly to the hosts PulseAudio server and uses it's configuration/devices to output the sound. This is achieved by mapping the UNIX socket used by PulseAudio in the host into the container and configuring its use.

Most of the complexity of the Dockerfile is setting up a non root user. This is kept in the example as near all uses of this **should** not be running the app as root.

## Common / Known Issues

### SHM
`shm_open() failed: No such file or directory` Is caused by PulseAudio trying to use Shared Memory (`/dev/shm` or `/run/shm`) as a performance enhancement. In this example SHM is disabled to prevent this issue.

### TCP / Avahi
This would be another method to configure PulseAudio by enabling the host server to open an TCP server and the container connecting to it. This was avoided as it:
 * Unnecessarily causes the container to be networked to the host.
 * Requires additional PulseAudio modules and changed configuration on the host.
 * Is less performant.
