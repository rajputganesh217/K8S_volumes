# K8S_volumes
 This repository contains multiple hands-on examples of using volume that  I have performed on cluster with steps and code files.


# Kubernetes Volume Practical Examples 🚀

This repository contains multiple hands-on examples of using volumes in Kubernetes for real-world use cases, including:

- `emptyDir` for intra-pod container communication
- `initContainer` with Git repo cloning
- `hostPath` volume mounting
- `NFS / Amazon EFS` shared volume setup
- `PersistentVolume (PV)` and `PersistentVolumeClaim (PVC)` with pods

---

## 📁 1. emptyDir Volume - `emptydir2.yml`

### 🔹 Purpose:
Shares a volume between two containers (`nginx` and `alpine`) in the same pod.

### 🔹 Behavior:
- Alpine container writes a dynamic `index.html` file every 5s.
- Nginx serves the same file using shared `emptyDir` volume.

### 🔹 How to Run:
```bash
kubectl apply -f emptydir2.yml
kubectl exec -it m-empty-dir -c nginx -- cat /usr/share/nginx/html/index.html
````

---

## 🧰 2. Git Repo Clone Using initContainer - `init-repo.yml`

### 🔹 Purpose:

Clone a GitHub repo into a pod volume using an `initContainer`.

### 🔹 Behavior:

* `initContainer` clones repo into a shared `emptyDir`.
* `nginx` container serves the repo contents.

### 🔹 How to Run:

```bash
kubectl apply -f init-repo.yml
kubectl logs gitpod -c git-clone
kubectl exec -it gitpod -- ls /usr/share/nginx/html
```

---

## 🏠 3. hostPath Volume with ReplicaSet - `hostpath.yml`

### 🔹 Purpose:

Mounts a host directory into each pod created by a ReplicaSet.

### 🔹 Behavior:

Each pod uses a directory from the worker node (`/home/ubuntu/myvolume`) and serves a static file.

### 🔹 How to Run:

```bash
# On worker node
sudo mkdir -p /home/ubuntu/myvolume
echo "Hello from hostPath volume" | sudo tee /home/ubuntu/myvolume/index.html

kubectl apply -f hostpath.yml
kubectl get pods -l app=facebook
```

---

## 📡 4. Amazon EFS as NFS Volume - `nfs.yml`

### 🔹 Purpose:

Mount an Amazon EFS (NFS) shared folder into a pod.

### 🔹 Behavior:

Pod runs NGINX, serving files directly from EFS.

### 🔹 How to Run:

```bash
# On worker node
sudo apt install -y nfs-common
sudo mkdir /mydata
sudo mount -t nfs4 <EFS-URL>:/ /mydata
sudo mkdir /mydata/test
echo "<h1>Hello from EFS</h1>" | sudo tee /mydata/test/index.html

# In cluster
kubectl apply -f nfs.yml
kubectl exec -it nfspod -- cat /usr/share/nginx/html/index.html
```

---

## 📦 5. PV, PVC, and Pod with hostPath - `pvhost.yml`, `pvchost.yml`, `pvchostpod.yml`

### 🔹 Purpose:

Demonstrates static volume provisioning using a hostPath PV and PVC.

### 🔹 Structure:

* `pvhost.yml` defines hostPath PersistentVolume
* `pvchost.yml` defines PersistentVolumeClaim
* `pvchostpod.yml` runs NGINX pod using PVC

### 🔹 How to Run:

```bash
sudo mkdir -p /home/ubuntu/myhost
echo "Hello from Worker Node" | sudo tee /home/ubuntu/myhost/about.html

kubectl apply -f pvhost.yml
kubectl apply -f pvchost.yml
kubectl apply -f pvchostpod.yml
```

---

## 📝 Notes

* All volumes used are either `emptyDir`, `hostPath`, or `nfs`.
* These examples are tested in a real Kubernetes cluster (Amazon EC2-based).
* `PersistentVolumeClaim` is used for static provisioning — for dynamic, use a StorageClass.

---

## 📌 Author

👨‍💻 Made by: Ganesh Rajput
 Kubernetes hands on  Collection – 2025

---

```

\
