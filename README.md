# LNMP

LNMP(Nginx, PHP, Mysql, Redis) service deploy through docker swarm mode.

## Swarm mode

```bash
docker swarm init
```

*Notice: if using command through `docker-compose`, you may make some changes to this `docker-compose.yaml` file to adjust some settings like `secret`*

## Usage

1. Create mysql password through docker secret

```bash
$ echo "my-secret-pw" | docker secret create mysql_root_password -
```

2. Deploy or update a stack

```bash
$ docker stack deploy -c docker-compose.yml lnmp
```
