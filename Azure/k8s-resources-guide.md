What is a Pod?
Smallest deployable unit in Kubernetes.

Can have one or more containers sharing storage, network, and namespace.

Used when you want a single container or tightly coupled containers to run together.

Key points about running a Pod directly (without Deployment)
It runs as a single instance

Only one Pod is created.

If it crashes or is deleted, Kubernetes will not automatically recreate it.

No scaling

You cannot easily run multiple replicas with a plain Pod.

For multiple replicas, you need a ReplicaSet or Deployment.

Mostly used for

Quick tests or experiments.

Debugging a container.

Learning purposes.

Lifecycle

Pod exists as-is until you delete it.

Once deleted, it’s gone.

No self-healing like Deployments.
