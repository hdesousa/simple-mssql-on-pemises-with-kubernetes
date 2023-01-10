# simple-mssql-on-pemises-with-kubernetes
## Simple on premises MSSQL server with kubernetes to use as SQL server in DEV environment

### Requirements
Docker Desktop
(https://www.docker.com/products/docker-desktop/) [https://www.docker.com/products/docker-desktop/]

### 1. Clone repository
```
git clone https://github.com/hdesousa/simple-mssql-on-pemises-with-kubernetes.git
```

### 2. Create a secret to store sa password
Put a password with a minimun of 8 and with special chars to avoid errors during the initialization
```
kubectl create secret generic mssql --from-literal=MSSQL_SA_PASSWORD="veryStrongP@ssword_1"
```

### 3. Edit mssqlPersistentVolume.yaml 
On line 7 change the size of the needed volume to store the SQL data.
It is configured to have 50 Gb
```
storage: 50Gi
```
On line 14 change the path of folder to save the volume.
Example on windows (to folder C:\persistentVolumes\mssql):
```
path: "/run/desktop/mnt/host/c/persistentVolumes/mssql"
```
Example on linux or mac (to folder /persistentVolumes/mssql):
```
path: "/persistentVolumes/mssql"
```

### 4. Edit mssqlPersistentVolumeClaim.yaml
On line 10 change the size to the same size inserted on line 7 of mssqlPersistentVolume.yaml
It is configured to have 50 Gb
```
storage: 50Gi
```

### 5. Edit mssqlDeployment.yaml
Change the development file with the needed parameters

### 6. (Just for windows) Pull manually the SQL image
Pull manually the SQL image (because a known bug on Kubernetes: [https://serverfault.com/questions/1107050/context-deadline-exceeded-error-on-pod-in-kubernetes-while-pulling-a-public-im] (https://serverfault.com/questions/1107050/context-deadline-exceeded-error-on-pod-in-kubernetes-while-pulling-a-public-im))
```
docker pull mcr.microsoft.com/mssql/server
```

### 7. Deploy server
Run this commands on terminal inside the repository folder:
```
kubectl apply -f .\mssqlPersistentVolume.yaml
```
```
kubectl apply -f .\mssqlPersistentVolumeClaim.yaml
```
```
kubectl apply -f .\mssqlDeployment.yaml
```
