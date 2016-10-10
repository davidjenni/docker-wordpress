# Dockerized WordPress with nginx
Describes a sample WordPress deployed as set of [docker containers](https://www.docker.com/what-docker) on a cloud VM, e.g. [DigitalOcean](https://digitalocean.com).
Each "concern" of a WordPress installation is factored into its separate docker container. This makes deployment, scaling and updating easier.
The separate containers are built and deployed using [docker-compose](https://docs.docker.com/compose/overview/).

## Create VM with docker-machine (non-swarmed)
- Using the [docker-machine DigitalOcean driver](https://docs.docker.com/machine/drivers/digital-ocean/),
create a VM named `do-sfo1-docker1` (choose your own naming scheme as desired):

```bash
$ docker-machine create --driver digitalocean
    --digitalocean-access-token $DO_TOKEN
    --digitalocean-image coreos-beta
    --digitalocean-region sfo1
    --digitalocean-size 512mb
    --digitalocean-ssh-user
    core do-sfo1-docker1
```
`$DO_TOKEN` is the personal access token for your DigitalOcean account and is assumed to be set as environment variable by the user.
Create and manage your account's access tokens at your [DO account settings](https://cloud.digitalocean.com/settings/api/tokens).

- Prime your cmd line to use this VM as target for docker commands:

```bash
$ docker-machine env do-sfo1-docker1
```

- Connect to your VM or directly execute commands:

```bash
$ docker-machine shh do-sfo1-docker1 docker -v
$ docker-machine shh do-sfo1-docker1
```

## Build containers and deploy
- Build and deploy:

```bash
$ docker-compose build
$ docker-compose run -d
```

- Determine IP address of the hosting VM:

```bash
$ docker-machine ip do-sfo1-docker1
```

- Connect to site and load index page
Either use your browser or simply use curl from the cmd line (using the -I flag to only show the headers):
```bash
$ curl -I http://<your-vm-ip-address>:8080/
HTTP/1.1 200 OK
Server: nginx/1.11.4
Date: Mon, 10 Oct 2016 01:09:20 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
X-Powered-By: PHP/7.0.11
```

- Check logs:

```bash
$ docker-compose logs
Attaching to dockerwordpress_nginx_1, dockerwordpress_phptest_1, dockerwordpress_phpfpm_1
nginx_1    | 50.125.229.141 - - [10/Oct/2016:01:09:20 +0000] "HEAD / HTTP/1.1" 200 0 "-" "curl/7.49.1"
nginx_1    | 2016/10/10 01:09:20 [info] 5#5: *13 client 50.125.229.141 closed keepalive connection
phpfpm_1   | [10-Oct-2016 00:16:12] NOTICE: fpm is running, pid 1
phpfpm_1   | [10-Oct-2016 00:16:12] NOTICE: ready to handle connections
phpfpm_1   | 172.18.0.4 -  10/Oct/2016:00:17:02 +0000 "GET /index.php" 200
phpfpm_1   | 172.18.0.4 -
```

