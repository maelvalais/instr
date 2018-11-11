# Remote docker

## Use docker-machine for remoting docker

```shell
docker-machine create --driver generic --generic-ip-address=141.115.74.15 --generic-ssh-key ~/.ssh/id_rsa --generic-ssh-user=mvalais polatouche-docker-host

eval $(docker-machine env polatouche-docker-host)
```

## Tunneling

    docker-machine ssh default -L 0.0.0.0:8000:localhost:8000
