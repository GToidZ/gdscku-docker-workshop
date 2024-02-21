# Containers Communication with Networks

In this exercise, you'll learn how to:
* Understand the underlying network layer between host and containers
* Create a new network

## Listing networks
Networks in Docker are groups of containers that can interoperate together and will have shared DNS functionality.

To list all Docker networks in your system,

```sh
$ docker network ls
NETWORK ID      NAME            DRIVER      SCOPE
99cbf6b3d074    bridge          bridge      local
...
```

By default, `bridge` is the network that Docker will assign any containers to. However, `bridge` does not allow containers to communicate with each other.

## Creating a network

1. To create a new network, use `docker network create` to do so,

   ```sh
   docker network create <network-name>
   ```

2. Check the network existence by using `docker network ls`.

## Adding containers to network

In order to see if our network works properly, we shall be using two PostgreSQL containers to communicate with one another. (Postgres binary provides `-h` flag to specify hostname.)

1. We'll start by spinning up two containers of PostgreSQL. **We need to also name those containers as they're used for default hostnames.**
   
   ```sh
   $ docker run --rm -d --name gdscdb --network <network-name> -e POSTGRES_PASSWORD=1234 -p 5432 postgres:latest

   $ docker run --rm -d --name kudb --network <network-name> -e POSTGRES_PASSWORD=1234 -p 5432 postgres:latest
   ```

2. We can now try to use one container to communicate to another. Let's try using `gdscdb`!

   ```sh
   $ docker exec -it gdscdb /bin/bash
   root@...> psql -U postgres -h kudb -p
   ```

   If `psql` can be launched, the network is working great!
