# Ingress Replication PoC

Proof of concept implementation of traffic replication using nginx based on [this guide](https://alibaba-cloud.medium.com/application-traffic-replication-through-a-kubernetes-ingress-controller-8da2b841d49d). Also demonstrates path rewriting in the replicated-ingress.

There are two nearly identical deployments and associated services and ingresses named `main-deployment` and `replicated-deployment`. The only substantial differences between these are in the ingress configurations which have explanatory comments of the key configuration.

This has been tested and found to work locally on minikube. You will need to run `minikube addons enable ingress` if you haven't yet enabled ingress on your minikube instance. 

Deploy with: 

```helm upgrade ingress-replication ./ --install```

Test the services are running with:
```helm test ingress-replication```

You can observe the replicated service logs with `kubectl logs deployment/replicated-deployment`. You should find that 
all requests sent to [http://main-deployment.local](http://main-deployment.local) are also logged here proving the principle.

If you call [http://replicated-deployment.local/something/damned/well](http://replicated-deployment.local/something/damned/well) you will see the url is transformed in the logs to `/whatever/we/damned/well/please/`. 

Because I've had to mirror directly to the service rather than the ingress we can't see both changes working at the same time but if we manage to mirror through the ingress the rewrite above will be applied. 