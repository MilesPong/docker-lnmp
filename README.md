# Run Laravel in Docker LNMP

An example to run Laravel application under LNMP(Nginx, PHP, Mysql, Redis) service supported by [docker](https://www.docker.com/).

## Important

**!!! Current `indigo` branch is only for project [indigo](https://github.com/MilesPong/indigo) usage, otherwise, you may want to switch to branch [`master`](https://github.com/MilesPong/docker-lnmp) !!!**

## Requirements

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Usage

### Configuration

```bash
$ git clone --recurse-submodules https://github.com/MilesPong/indigo.git
$ cd indigo
$ cp .env.example .env
```

Modify file `.env` as follow:

```
# Default port is 8000
APP_URL=http://localhost:8000
# Database setting
DB_HOST=mysql
DB_DATABASE=laravel_indigo
DB_USERNAME=root
DB_PASSWORD=root
# Log channel
LOG_CHANNEL=single
```

Change redis' host to `redis`, e.g. `REDIS_HOST=redis`.

Also, don't forget to set up the remain configurations as [mentioned](https://github.com/MilesPong/indigo/blob/master/README.md#installation).

### Install Dependencies

Install dependencies via official composer docker:

```bash
$ docker run -u $(id -u):$(id -g) --rm -v $(pwd):/app composer install
```

Compiling assets:

```bash
$ docker run --rm -u $(id -u):$(id -g) -v $(pwd):/app -w /app milespeng/node:alpine npm install
$ docker run --rm -u $(id -u):$(id -g) -v $(pwd):/app -w /app milespeng/node:alpine npm run dev
$ docker run --rm -u $(id -u):$(id -g) -v $(pwd):/app -w /app milespeng/node:alpine npm run admin-dev
```

### Ready to Go

Boot the services:

```bash
$ docker-compose -f docker/docker-compose.yml up -d
```

Generate application key:

```bash
$ docker-compose -f docker/docker-compose.yml run --no-deps --rm -w /app php72 php artisan key:generate
```

Create a symbolic link:

```bash
$ docker-compose -f docker/docker-compose.yml run --no-deps --rm -w /app php72 php artisan storage:link
```

Run migrations and seeders:

```bash
$ docker-compose -f docker/docker-compose.yml run --no-deps --rm -w /app php72 php artisan migrate --seed
```

Create the first user:

```bash
$ docker-compose -f docker/docker-compose.yml run --no-deps --rm -w /app php72 php artisan user:add
```

(**Optional**) Import fake data for development:

```bash
$ docker-compose -f docker/docker-compose.yml run --no-deps --rm -w /app php72 php artisan db:seed --class=FakeDataSeeder
```

Now if everything is going right, you can access the application via [http://localhost:8000](http://localhost:8000).

## TODO

Laravel **schedule** is not supported yet, but  it seems to be unnecessary for development.

Also, you may want to modify `.env` as follow to make **queue** "work" in docker.

```
QUEUE_DRIVER=sync
```