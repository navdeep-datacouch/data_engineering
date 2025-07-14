# Lab: Persist PostgreSQL Data Using PVC with Flask App on K3s

## Step-by-step Instructions

## ☘️ Pre-requiste : Setup K3s Cluster
1. Stop Minikube
```bash
minikube stop
minikube delete
```


1. Run below to start k3s cluster

```bash
cd ~/data_engineering/Kubernetes/Lab6
chmod 777 runk3s.sh
./runk3s.sh
export KUBECONFIG=$HOME/k3s.yaml
```

### Step 1: Check existing storage classes
```bash
kubectl get storageclasses
```

### Step 2: Create PVC with Storage Class

```bash
cd ~/data_engineering/Kubernetes/Lab6
kubectl apply -f postgres-pv-pvc.yaml
```

### Step 3: Check  PVC
```bash
kubectl get pvc
```

### Step 4: Deploy PostgreSQL with PVC

Explore postgres-deployment.yaml and check how PVc is added into deployment

Now apply postgres deployment

```bash
kubectl apply -f postgres-deployment.yaml
```
Expose it via Service 

```bash
kubectl apply -f postgres-service.yaml
```

Check postgres Pod and Service

```bash
kubectl get pod
kubectl get svc
```
Make sure Postgres Pod is in running status before moving ahead

### Step 3: Deploy Flask App and Expose it via Service

We have not done any changes in flask deployment and flask service, it is same as in last lab.

Explore flask-deployment.yaml and flask-service.yaml if you want

```bash
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
```

Check Flask Pod and Service

```bash
kubectl get pod
kubectl get svc
```

Make sure Flask Pod is in running status before moving ahead

```
### 🔍 Open your local browser and go to:
Replace the EC2-Address that you have recieved in last command in below URL

  👉 `http://localhost:30000`

---

## ☘️ Step 10: Test the App

- Fill in a **username** and **age**
- Click **Submit**
- You should see:  
  `Thank you, <name>! Your age <age> has been recorded.`

---

### Step 11: Test Data Persistence

```bash
kubectl delete pod -l app=postgres
```

Then access the postgres database and check if previous data persists.

## ☘️ Step 11: Verify DB

Inside terminal, connect to the postgres pod:

```bash
kubectl exec -it <postgres-pod-name> -- psql -U flaskuser -d flaskdb
```

Then run:

```sql
SELECT * FROM users;
```

---

## ☘️ Step 12: Cleanup

```bash
cd ~/data_engineering/Kubernetes/Lab6
kubectl delete -f .
```

🎉 **Well done!** You've now built a persistent App on kubernetes.  
✨ **END OF LAB** ✨
