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

