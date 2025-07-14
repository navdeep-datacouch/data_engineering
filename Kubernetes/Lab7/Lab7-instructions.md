# Lab: Use ConfigMap for PostgreSQL Credentials in Flask App

## 🧩 Objective

You will:
- Create a ConfigMap and Secret to store PostgreSQL connection details.
- Inject values into PostgreSQL and Flask pods.
- Test Application

---

## ☘️ Pre-requiste : Setup K3s Cluster
1. Stop Minikube
```bash
minikube stop
minikube delete
```


1. Run below to start k3s cluster

```bash
cd ~/data_engineering/Kubernetes/Lab7
chmod 777 runk3s.sh
./runk3s.sh
export KUBECONFIG=$HOME/k3s.yaml
```

## 🚀 Step-by-Step Guide

### Step 1: Apply ConfigMap

```bash
cd ~/data_engineering/Kubernetes/Lab7
kubectl apply -f configmap.yaml
```

Check Configmap

```bash
kubectl get configmap
```

### Step 2: Create Secret for Postgres Credentials
```bash
kubectl create secret generic postgres-secret \
  --from-literal=DB_USER=flaskuser \
  --from-literal=DB_PASSWORD=flaskpass
```
Check Secrets
```bash
kubectl get secret
```

### Step 3: Create PVC with Storage Class

```bash
cd ~/swift_training/Lab6
kubectl apply -f postgres-pv-pvc.yaml
```

### Step 4: Deploy PostgreSQL

```bash
kubectl apply -f postgres-deployment.yaml
kubectl apply -f postgres-service.yaml
```
Check Postgres Pod & Service
```bash
kubectl get pod
kubectl get svc
```


### Step 5: Deploy Flask App

```bash
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
```

Check Flask Pod & Service
```bash
kubectl get pod
kubectl get svc
```

### Step 6: Connect to Flask Pod

Connect to Flask Pod terminal 
```bash
kubectl exec -it flask-pod-name -- bash
```
Once inside the pod, check for environment variables that you have set in configmap
```bash
env
```

Once inside the pod, check for mounted secret
```bash
cat /mnt/secrets/DB_USER
cat /mnt/secrets/DB_PASSWORD
```

Exit the pod
```bash
exit
```

## ☘️ Step 7: Access the Flask App
```
### 🔍 Open your local browser and go to:
Replace the EC2-Address that you have recieved in last command in below URL

  👉 `http://localhost:30000`

---

## ☘️ Step 8: Test the App

- Fill in a **username** and **age**
- Click **Submit**
- You should see:  
  `Thank you, <name>! Your age <age> has been recorded.`

---

## 🧹 Cleanup

```bash
cd ~/data_engineering/Kubernetes/Lab7
kubectl delete -f .
```
