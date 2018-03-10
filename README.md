LNMP
=====

## Prepare

```bash
docker swarm init
```

## Usage

1. Create mysql password through docker secret

```bash
cp mysql/mysql_root_password.secret.exmpale mysql/mysql_root_password.secret
# change the password if you need
```

2. Build and start containers

```bash
docker-compose up -d --build
```
