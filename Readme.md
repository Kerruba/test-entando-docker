# Readme

The idea behind this repo is to provide a docker test environment for an Entando app using *Spring data* for the Digital Exchange plugin

# Requirements

- Docker
- entando-components on branch **EN-2382-Jobs_endpoint**

# How to test this repo

First you need to build locally both `entando-core` and `entando-components` repositories

## Entando core:

```
mvn clean install -DskipTests
```

## Entando components
```bash
# If you're still on the master branch switch to EN-2382-Jobs_endpoint
git checkout EN-2382-Jobs_endpoint

mvn clean install -DskipTests
```

## Run the application using wildfly profile

```bash

cd test1-entando-docker

# Build docker images and run the containers
mvn clean install -Pwildfly -Ppostgres docker:run

```

## Check the ports on docker
In order to interact with the container you need to get it's local 
port binding. To check this use

```bash
docker container ls
```

Under the `PORTS` column you will find the bindings. Here an example of mine

```bash
CONTAINER ID        IMAGE                                            COMMAND                  CREATED             STATUS              PORTS                                              NAMES
e2833bbdd660        entandosamples/test1:5.1.0-SNAPSHOT              "container-entrypoin…"   3 hours ago         Up 3 hours          172.17.0.1:32792->8080/tcp                         test1-engine
30b039136491        entandosamples/test1-db                          "container-entrypoin…"   3 hours ago         Up 3 hours          172.17.0.1:5432->5432/tcp                          test1-dbms
0d1549cf2e34        entando/appbuilder:5.1.0-SNAPSHOT                "container-entrypoin…"   3 hours ago         Up 3 hours          8080/tcp, 172.17.0.1:32793->5000/tcp               stoic_einstein
```

The one you should use is `entandosamples/test1:5.1.0-SNAPSHOT`

## Test the instance
You can now interact with the instance using any service, e.g. Postman, and register a Digital Exchange to this. For semplicity in this case
you can register as Digital Exchange the Entando test instance.


- Components should be installable
- `/digitalExchange/jobs` endpoint should work and accept filters
