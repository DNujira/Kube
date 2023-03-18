# Kube

# ขั้นตอนการติดตั้ง

<details>
  <summary><h3>ขั้นตอนการติดตั้ง kubectl</h3></summary>
  
### REF : https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

**1. Download โดยใช้คำสั่งด้านล่างนี้**
```
curl.exe -LO "https://dl.k8s.io/release/v1.26.0/bin/windows/amd64/kubectl.exe"
```
**2. เพิ่ม path ของ kubectl ใน Environment variables**

**3. ทดสอบว่า kubectl ทำงานหรือไม่โดยใช้คำสั่ง**
```
kubectl version --client
kubectl version --client --output=yaml  //ข้อมูลแบบไฟล์ yaml
```
![image](https://user-images.githubusercontent.com/117592447/226132590-8ba8fefd-6eb5-458d-9c90-52896167b3c4.png)

</details>

<details>
  <summary><h3>ขั้นตอนการติดตั้ง minikube</h3></summary>
  
  ### REF : https://minikube.sigs.k8s.io/docs/start/
**1. เข้าไปในเว็บไซต์และเลือก spec ให้ตรงกับเครื่องของเรา หลังจากนั้นจะมีคำสั่งติดตั้งมาให้**

![image](https://user-images.githubusercontent.com/117592447/226132789-eb69af64-301b-40cc-bca2-4f04d9bfbc03.png)

**เข้าไปที่ powershell และใช้คำสั่งด้านล่างนี้ในการติดตั้ง**
```
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```
**2. นำไฟล์ minikube.exe ที่ download มาไปไว้ใน path**

![image](https://user-images.githubusercontent.com/117592447/226133501-dd44ea6c-740b-4205-a2aa-fb0412cd065b.png)

**หรือใช้คำสั่งด้านล่างนี้ใน powershell**
```
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ `
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
}
```
**3. ทดสอบว่า minikube ใช้งานได้หรือไม่ โดยการพิมพ์คำสั่งด้านล่างนี้ใน CMD**
```
minikube
```
ถ้าขึ้น ดังรูป แสดงว่า minikube ใช้งานได้แล้ว

![image](https://user-images.githubusercontent.com/117592447/226133683-489e7508-8e75-4544-b3f8-8f31a85924f5.png)

 ## ตัวอย่างการใช้ minikube Deploy applications(Service)
  ### REF : https://minikube.sigs.k8s.io/docs/start/
  
  ![image](https://user-images.githubusercontent.com/117592447/226139952-071ce784-6d54-41f2-a79f-4c9255b601e9.png)

  
  **คำสั่ง deploy**
  ```
  kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
  kubectl expose deployment hello-minikube --type=NodePort --port=8080
  ```
  **ผลลัพธ์**
  
  ![image](https://user-images.githubusercontent.com/117592447/226139909-f5580cb0-33d6-43b4-8955-199065040596.png)

  </details>
  
  <details>
  <summary><h3>ขั้นตอนการติดตั้ง docker desktop</h3></summary>
  
  ### REF : https://docs.docker.com/desktop/install/windows-install/
  
  **กดที่ Docker Desktop for Windows**
  
  ![image](https://user-images.githubusercontent.com/117592447/226133923-c637f01d-1f4b-414a-ae03-b64c81f3d2b7.png)
 หลังจากติดตั้งและทดลองนำ minikube มารันโดยใช้คำสั่งด้านล่างนี้
 
 **REF : https://minikube.sigs.k8s.io/docs/drivers/docker/**
 ```
 minikube start --driver=docker
```
**ผลลัพธ์เมื่อลอง run minikube แล้ว**

![image](https://user-images.githubusercontent.com/117592447/226134145-273fab47-b6f0-4bb1-a11e-83d5ec1c3a7a.png)

![image](https://user-images.githubusercontent.com/117592447/226134156-7d03411a-5bac-47f7-b972-be627c7412fa.png)

  </details>
  
  
# Traefik Deploy
  
  ### REF : https://github.com/iamapinan/kubeplay-traefik
  **เพิ่ม 127.0.0.1 traefik.spcn18.local ในไฟล์ที่ชื่อ host ที่ path C:\Windows\System32\drivers\etc**
  
  ![image](https://user-images.githubusercontent.com/117592447/226135476-fd1f6f17-6e02-456d-b330-2c2f02d8641f.png)

  **1. Install Traefik โดยใช้คำสั่ง**
  ```
  kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.9/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml
  ```
  **2. Install RBAC for Traefik โดยใช้คำสั่ง**
  ```
  kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.9/docs/content/reference/dynamic-configuration/kubernetes-crd-rbac.yml
  ```
  **3. สร้าง namespace โดยใช้คำสั่ง**
  ```
  kubectl create namespace spcn18
  ```
  **4. Install Traefik Helmchart โดยใช้คำสั่ง**
  ```
  helm repo add traefik https://traefik.github.io/charts
  helm repo update
  helm install traefik traefik/traefik
  ```
  **5. ตรวจสอบว่า service run อยู่หรือไม่ โดยใช้คำสั่ง**
  ```
  kubectl get svc -l app.kubernetes.io/name=traefik
  kubectl get po -l app.kubernetes.io/name=traefik
  ```
  **ผลลัพธ์**
  ![image](https://user-images.githubusercontent.com/117592447/226135109-6c677364-7f6d-41cb-8e14-0c779f5c8ed8.png)
  
  **6. สร้าง tunnel เพื่อใช้เป็น EXTERNAL-IP โดยใช้คำสั่ง**
  ```
  minikube tunnel
  ```
  **7. สร้างไฟล์ secret โดยใช้คำสั่งด้านล่าง (run ใน bash)**
  ```
  htpasswd -nB user | tee auth-secret //ตรง user สามารถเปลี่ยนได้
  ```
  ```
  kubectl create secret generic -n กำหนดชื่อ dashboard-auth-secret \
   --from-file=users=auth-secret -o yaml --dry-run=client | tee dashboard-secret.yaml
  ```
  ![image](https://user-images.githubusercontent.com/117592447/226135981-4f1b21eb-f62a-49c9-8237-3142b5e29341.png)

  **เมื่อ run เสร็จแล้วจะไฟล์ dashboard-secret.yaml มา และนำข้อมูลตรง user ไปใส่ในไฟล์ traefik-dashboard.yaml ให้ตรงกัน**
  
  ![image](https://user-images.githubusercontent.com/117592447/226136289-d53718c2-59cf-41e1-8367-cd7829915f59.png)
  ![image](https://user-images.githubusercontent.com/117592447/226136168-1fded7f3-a7dc-4354-8cb9-28ee63495cfd.png)

  **8. Deploy โดยใช้คำสั่ง**
  ```
  kubectl apply -f traefik-dashboard.yaml
  ```
  **ทดสอบว่า deploy traefik สำเร็จหรือไม่โดยการใช้ domain ที่ตั้งไว้ (https://traefik.spcn18.local/dashboard/#/)**
  **ผลลัพธ์**
  
  ![image](https://user-images.githubusercontent.com/117592447/226136393-f9b4be39-76b9-4c45-bb4b-659bad6524de.png)

  </details>
 
<br>

# deploy rancher/hello-world
**เพิ่ม 127.0.0.1 web.spcn18.local ในไฟล์ที่ชื่อ host ที่ path C:\Windows\System32\drivers\etc**

![image](https://user-images.githubusercontent.com/117592447/226137281-6c14884b-5f29-4997-8f18-d0e893a436d2.png)

  **1. สร้างไฟล์ rancher-hello-world.yaml และเพิ่มโค้ดด้านล่างลงไปในไฟล์**
<details>
  <summary>CODE</summary>
  
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rancher-deployment
  namespace: spcn18
spec:
  replicas: 1
  selector:
    matchLabels:
      app: rancher
  template:
    metadata:
      labels:
        app: rancher
    spec:
      containers:
      - name: rancher
        image: rancher/hello-world
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: rancher-service
  labels:
    name: rancher-service
  namespace: spcn18
spec:
  selector:
    app: rancher
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 80
```
</details>

 **2. สร้างไฟล์ service.yaml และเพิ่มโค้ดด้านล่างลงไปในไฟล์**
<details>
  <summary>CODE</summary>
  
```
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: service-ingress
  namespace: spcn18
spec:
  entryPoints:
    - web
    - websecure
  routes:
  - match: Host(`web.spcn18.local`)
    kind: Rule
    services:
    - name: rancher-service
      port: 80

```
</details>

**3. deploy ไฟล์ rancherhello-world.yaml**
```
kubectl apply -f rancherhello-world.yaml     
```
**4. deploy ไฟล์ service.yaml**
```
kubectl apply -f service.yaml
```
**5. สร้าง tunnel โดยใช้คำสั่ง**
```
minikube tunnel
```
 **ทดสอบว่า deploy สำเร็จหรือไม่โดยการใช้ domain ที่ตั้งไว้ (http://web.spcn18.local/)**
 
  **ผลลัพธ์**
  ![image](https://user-images.githubusercontent.com/117592447/226138556-9a410c45-271b-4815-9f01-da736caaaf04.png)
