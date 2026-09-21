# Kubernetes Deployment Instructions

## 1. Create and Select the Namespace

Go to the `.infrastructure` directory:

```bash
cd .infrastructure
```

Create the Kubernetes namespace:

```bash
kubectl apply -f namespace.yml
```

This command creates the `todoapp` namespace.

Set `todoapp` as the default namespace for the current Kubernetes context:

```bash
kubectl config set-context --current --namespace=todoapp
```

After this, you can run `kubectl` commands without specifying `-n todoapp`.

---

## 2. Deploy the Pods

Apply the BusyBox pod manifest:

```bash
kubectl apply -f busybox.yml
```

Apply the ToDo application pod manifest:

```bash
kubectl apply -f todoapp-pod.yml
```

Check that the pods are running:

```bash
kubectl get pods
```

---

## 3. Deploy the Services

Create the ClusterIP service:

```bash
kubectl apply -f clusterIp.yml
```

Create the NodePort service:

```bash
kubectl apply -f nodePort.yml
```

Check the created services:

```bash
kubectl get services
```

---

## 4. Test the Services from the BusyBox Pod

Open a shell inside the BusyBox pod:

```bash
kubectl exec -it busybox -- sh
```

Inside the BusyBox pod, test the NodePort service through Kubernetes DNS:

```bash
curl http://todoapp-nodeport-service.todoapp.svc.cluster.local
```

Test the ClusterIP service:

```bash
curl http://todoapp-clusterip-service.todoapp.svc.cluster.local
```

Both commands should return the HTML of the ToDo application page.

Exit the BusyBox shell:

```bash
exit
```

---

## 5. Access the Application Through the NodePort Service

The NodePort service exposes the application on port `30008`.

Open the following URL in a browser:

```text
http://localhost:30008/
```

> Note: direct access through `localhost:30008` depends on the local Kubernetes environment and how NodePort networking is exposed by the cluster.

---

## 6. Test the ClusterIP Service with Port Forwarding

In a separate terminal, forward local port `30009` to port `80` of the ClusterIP service:

```bash
kubectl port-forward service/todoapp-clusterip-service 30009:80
```

Then open the application in a browser:

```text
http://localhost:30009/
```

This forwards traffic from your local machine to the ClusterIP service inside the Kubernetes cluster.
