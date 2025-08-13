**What is a Pod?**
Smallest deployable unit in Kubernetes.

Can have one or more containers sharing storage, network, and namespace.

Used when you want a single container or tightly coupled containers to run together.

***Key points about running a Pod directly (without Deployment)*
1.It runs as a single instance

Only one Pod is created.

If it crashes or is deleted, Kubernetes will not automatically recreate it.

2.No scaling

You cannot easily run multiple replicas with a plain Pod.

For multiple replicas, you need a ReplicaSet or Deployment.

3.Mostly used for

Quick tests or experiments.

Debugging a container.

Learning purposes.

4.Lifecycle

Pod exists as-is until you delete it.

Once deleted, it’s gone.

No self-healing like Deployments.

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b130fa4d-d5bb-4623-b9b3-c30420ad6479" />


**2  ReplicaSet**
What is a ReplicaSet?
Ensures that a specified number of pod replicas are running at all times.

Automatically creates or deletes pods to match the desired replica count.

Usually not used directly in production — Deployments manage ReplicaSets for you.


