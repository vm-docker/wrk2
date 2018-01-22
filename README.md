TASKS
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Steps:
1. Find any open source web application that you like, written in Go, Python or PHP.
2. Implement a `Dockerfile` to dockerize this application (or use existing one).
3. Implement a `docker-compose.yaml` spec to run this application in dockerized environment. 

Requirements:
- Everything is dockerized.
- Getting whole application up with single `docker-compose up`.

Following will be appreciated:
- Kubernetes spec of application (in addition to `docker-compose.yaml` spec).
- e2e tests which verify that application is running okay in dockerized environment.
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

SOLUTIONS
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

1.We have a Dockerfile, which deploy opencart-lamp(Linux, Apache2, MySQL, PHP5)

2. Put Dockerfile into directory /wrk2

3. Implement docker-compose.yaml and put into /wrk2

-ARCHITECTURE :

The whole application is divided into two Containers:

Apache & PHP5  is running in fpm Container, which handles requests, makes responses, retrieves php scripts from host, interprets, executes then responses to Apache. If necessary, it will connect to MySQL as well.
MySQL lies in mysql Container,
My opencart application scripts are located on host, you can edit files directly without rebuilding/restarting whole images/containers.

-BUILD AND RUN

Without building images one by one, you can make use of docker-compose and simply issue:

$ sudo docker-compose up

You need to check permission of opencart folders. or chmod -R 777 opencart & chown -R www-data:www-data opencart

-TESTING

a) simple test , using curl

$ cd test

$ ./test.sh

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 40718    0 40718    0     0  69251      0 --:--:-- --:--:-- --:--:-- 69248
Tests passed!

b) using docker-compose.test.yml 

$ docker-compose -f docker-compose.test.yml -p ci build

$ docker-compose -f docker-compose.test.yml -p ci up -d

ci_redis_1 is up-to-date
ci_mysql_1 is up-to-date
ci_fpm_1 is up-to-date
Starting ci_sut_1 ... done

$ docker logs -f ci_sut_1

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 40718    0 40718    0     0  87321      0 --:--:-- --:--:-- --:--:-- 87377
Tests passed!

