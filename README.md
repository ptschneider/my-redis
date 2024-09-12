# my-redis

Redis had a resurgence with the GenAI bubble as a vector database.

I haven't used it that way, I used it as a basic key-value.

## Config

No free upgrade past 7.2.0-v12 redis, changed license in 7.4.0-v0 to non-FOSS

For custom config, add a volume assignment for docker-compose, e.g.:

```
    volumes:
      - /work/pschneider/redis-stack-server/myredisdata:/data
      - /work/pschneider/redis-stack-server/my-redis-stack.conf:/redis-stack.conf
```


 After it is running, run the following command to get a command prompt
``` 
docker exec -it my-redis redis-cli
```

You do not need to install redis-cli on your local machine, just run the one alredy in the container.

## History

The one project I used Redis on was large, prod app would have about 40-60 docker containers running, most providing unique services and not just doing scale-out.

When working on a component it would be highly variable what other services would need to be running to test a feature. You might need 3 containers up, you might need 30, depended entirely on the ticket details. Between dev, int, qa, and prod environments, there were a near-infinite number of potential topologies that were potentially 'valid' at any given point in time. 

In that context, we used redis as a startup coordinator/discovery service; everything could run standalone, but have degraded function until it connected with other containers. Environment/topology of distributed system state was bootstrapped via redis. We didn't use it for anything else, we had strong mongo & neo4j backends. If you missed starting a service, it was easy to add to the running group, it got any needed metadata about its peers from the redis instance. 

Not a particularly complicated use-case, yet for internal orchestration of a complex app, redis was easy to live with.

