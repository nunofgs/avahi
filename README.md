# Avahi

This is a Dockerfile to set up [Avahi](http://www.avahi.org).

This container will run an avahi daemon and broadcast services based on other containers.

Dynamically generating services is possible by exporting the following environment variables:

| Variable     | Description                                  |
|--------------|----------------------------------------------|
| SERVICE_NAME | The name of the service broadcasted by avahi |
| SERVICE_PORT | The port where the service is located        |
| SERVICE_TYPE | The type of service                          |

# Usage

First, boot up the container you intend to broadcast the service from. For example, a [samba](github.com/dperson/samba) container:

```shell
$ docker run \
  -e "SERVICE_NAME=MyServer"
  -e "SERVICE_PORT=445"
  -e "SERVICE_TYPE=_smb._tcp"
  -p 137:137
  -p 138:138
  -p 139:139
  -p 445:455
  -v /mnt/data:/data
  dperson/samba
```

Next, start up the *avahi* container which will generate the services dynamically:

```shell
$ docker run \
  --net=host
  -v /var/run/docker.sock:/tmp/docker.sock:ro
  nunofgs/avahi
```

# Thanks

A special thank you to [nginx-proxy](github.com/jwilder/nginx-proxy) which this project is based on.
