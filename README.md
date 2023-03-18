# Kube

## ขั้นตอนการติดตั้ง

<details>
  <summary><b>ขั้นตอนการติดตั้ง kubectl</b></summary>
  
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
  <summary><b>ขั้นตอนการติดตั้ง minikube</b></summary>
  
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

  </details>
  
  <details>
  <summary><b>ขั้นตอนการติดตั้ง docker desktop</b></summary>
  
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
  
  
  <details>
  <summary><b>Traefik Deploy</b></summary>
  
  ### REF : https://github.com/iamapinan/kubeplay-traefik
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
  
  **6. สร้าง tunnel เพื่อ
  </details>
