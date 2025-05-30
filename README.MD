# Advanced Programming Module 11 - Deployment
### Athazahra Nabila Ruby - 2306173113

## Hello Minikube
> Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?
 
![Before](images/before.png)
Before exposing the deployment using kubectl expose, the application logs only showed the initial messages, indicating that the container was running and listening but had not received any traffic.

![After](images/after.png)
After exposing it and accessing the URL, 2 new lines appeared. These log entries show that the service received HTTP GET requests, meaning the application was accessed from a browser or HTTP client.

![After 2](images/after2.png)
I tried opening the service multiple times, and with each refresh, more GET / logs appeared, confirming that the pod was receiving and handling requests successfully.

> Notice that there are two versions of kubectl get invocation during this tutorial section. The first does not have any option, while the latter has -n option with value set to kube-system. What is the purpose of the -n option and why did the output not list the pods/services that you explicitly created?
 
- kubectl get pods,services shows the pods and services in the default namespace.
- kubectl get pods,services -n kube-system lists resources in the kube-system namespace, such as DNS and Metrics Server.
 
The -n flag is used to specify the namespace. Without it, kubectl shows resources only in the current or default namespace.


## Rolling & Kubernetes Manifest File
>  What is the difference between Rolling Update and Recreate deployment strategy?
 
- Rolling Update = updates pods gradually, ensuring that some pods of the old version are still available while the new ones start running. This minimizes downtime and keeps the service accessible during updates.
- Recreate = terminates all existing pods first, then starts new ones. This may cause temporary downtime but it's simpler and may be used when old and new versions can’t run side by side.

> Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.
 
I modified the deployment YAML by changing the strategy type to:
```
yaml
Copy
Edit
strategy:
  type: Recreate
```
![Recreate](images/recreate.png)
After applying the updated manifest with kubectl apply -f deployment.yaml, I observed in kubectl get pods that all previous pods were terminated before the new ones started, showing the behavior of the Recreate strategy.

> Prepare different manifest files for executing Recreate deployment strategy.
 
Available on the repository as 'deployment-recreate.yaml'

> What do you think are the benefits of using Kubernetes manifest files?
 
Kubernetes manifest files make deployments more consistent, reproducible, and easier to manage. Instead of manually typing commands, I can apply changes reliably using kubectl apply -f, which reduces the chance of errors. They're also easier to track with version control and integrate into automation tools, making the deployment process smoother and more efficient.

> BONUS: Deploy on a Managed Kubernetes Cluster (Google Cloud Platform - GKE)

![GKE](images/gke.png)
Firstly, I created an account on Google Cloud Platform and created a new a project 'spring-petclinic-project'.

![Cluster](images/cluster.png)
I then set up the Google Cloud SDK, selected my project, created the GKE cluster, then connected kubectl to GKE.

![Deploy](images/deploy.png)
Next, I used the same deployment.yaml file as in the Minikube setup to deploy it.

![Expose](images/expose.png)
![Expose 2](images/expose2.png)
Lastly, I exposed the app: http://34.126.167.250/petclinic

Reflection: Using GKE took more time to set up compared to Minikube because I had to create the cluster and configure everything on Google Cloud. However, GKE made it easier to scale and access the app publicly using a LoadBalancer. The kubectl commands were the same in both environments once I connected to the GKE cluster, which was convenient. I also noticed that I didn’t have to worry about managing the master nodes on GKE, unlike Minikube where everything runs locally. While Minikube was faster and free to use, GKE felt more like a real production environment, but the downside is that it comes with usage costs after my free trial ends.