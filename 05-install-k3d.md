
# K3d 


เป็นการจำลอง Kubernetes Cluster ให้ทำงานผ่าน Container ได้ ภายในเครื่องของเราเอง


K3d เป็น Wrapper ของ k3s 


k3d ใช้ Docker Containers ในการจำลอง Nodes ของ K3s:

- รันใน Docker: k3d สร้าง Docker Containers ขึ้นมาเพื่อทำหน้าที่เป็น Control Plane Node (Master) และ Worker Nodes สำหรับคลัสเตอร์ K3s
- รวดเร็วและเบา: เนื่องจาก K3s มีขนาดเล็กและรันอยู่ใน Docker Container ทำให้การสร้าง และการทำลาย Cluster ทำได้เร็ว

# Installing


[bookmark](https://github.com/bomb0069/introduction-to-k8s-lab/tree/main/01-install)


# K3d Command


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/cca714efa2c394ab91ea8381ad11d6d6.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=VV7cXcT97tnZqSGH5UQSNVdrLWzG4e6jwqFeotj%2FvOL6DZ3%2BVLsax8SNcQIEHAuuW0TfSPgorlC1MUXPcTYkZ4pcOZFVGCsKvt7G%2BRHVBDZNy4oUdwwQKkgEFugq0Eg7Ooz59W8yH59dkugnrg0dGUrLxFsXsfT29hM32ZdOhNt%2Fkb%2FWKbUHV%2BesfxwFX%2BrcF22TzvHFjI7CUba8YQGdEtdNNU33QRA0pd4xWM4CmCX8geoYyKS6G%2BQ%2BAmqqKKE2QPbNzTXd3HNXzyhlrGQmXdwEkrQ%2FNPDCkiD8WxJllWc97J2U8D5IotDv98pNxX0N7%2B%2FutFz9z1j2XH7hLiM6qg%3D%3D)


## Create Cluster


```bash
k3d cluster create localk8s

INFO[0000] Prep: Network
INFO[0000] Created network 'k3d-localk8s'
INFO[0000] Created image volume k3d-localk8s-images
INFO[0000] Starting new tools node...
INFO[0001] Creating node 'k3d-localk8s-server-0'
INFO[0001] Pulling image 'ghcr.io/k3d-io/k3d-tools:5.8.3'
INFO[0003] Pulling image 'docker.io/rancher/k3s:v1.31.5-k3s1'
INFO[0004] Starting node 'k3d-localk8s-tools'
INFO[0011] Creating LoadBalancer 'k3d-localk8s-serverlb'
INFO[0012] Pulling image 'ghcr.io/k3d-io/k3d-proxy:5.8.3'
INFO[0016] Using the k3d-tools node to gather environment information
INFO[0016] Starting new tools node...
INFO[0017] Starting node 'k3d-localk8s-tools'
INFO[0018] Starting cluster 'localk8s'
INFO[0018] Starting servers...
INFO[0018] Starting node 'k3d-localk8s-server-0'
INFO[0022] All agents already running.
INFO[0022] Starting helpers...
INFO[0022] Starting node 'k3d-localk8s-serverlb'
INFO[0029] Injecting records for hostAliases (incl. host.k3d.internal) and for 3 network members into CoreDNS configmap...
                                               INFO[0031] Cluster 'localk8s' created successfully!
                           INFO[0032] You can now use it like this:         
       kubectl cluster-info
```


```bash
# cluster info
kubectl cluster-info
Kubernetes control plane is running at https://0.0.0.0:43313
CoreDNS is running at https://0.0.0.0:43313/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:43313/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```


```bash
# delete cluster
k3d cluster delete <cluster-name>

# list node
kubectl get node
```


### Create Cluster with specific Master/Worker node


```bash
k3d cluster create <cluster-name> \
	--servers <number-of-master-node> \
	--agents <number-of-worker-node>
	
# Example
$ k3d cluster create my-k8s --servers 1 --agents 2
INFO[0000] Prep: Network
INFO[0000] Created network 'k3d-my-k8s'
INFO[0000] Created image volume k3d-my-k8s-images
INFO[0000] Starting new tools node...
INFO[0000] Starting node 'k3d-my-k8s-tools'
INFO[0001] Creating node 'k3d-my-k8s-server-0'
INFO[0001] Creating node 'k3d-my-k8s-agent-0'
INFO[0001] Creating node 'k3d-my-k8s-agent-1'
INFO[0001] Creating LoadBalancer 'k3d-my-k8s-serverlb'
INFO[0001] Using the k3d-tools node to gather environment information
INFO[0001] Starting new tools node...
INFO[0001] Starting node 'k3d-my-k8s-tools'
INFO[0003] Starting cluster 'my-k8s'
INFO[0003] Starting servers...
INFO[0003] Starting node 'k3d-my-k8s-server-0'
INFO[0007] Starting agents...
INFO[0007] Starting node 'k3d-my-k8s-agent-1'
INFO[0007] Starting node 'k3d-my-k8s-agent-0'
INFO[0010] Starting helpers...
INFO[0010] Starting node 'k3d-my-k8s-serverlb'
INFO[0016] Injecting records for hostAliases (incl. host.k3d.internal) and for 5 network members into CoreDNS configmap...
INFO[0019] Cluster 'my-k8s' created successfully!
INFO[0019] You can now use it 
```


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/9c6a457ca2de23b6bc63c648b85f75da.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=TXsiX6%2BLxs2%2BDzt0D1u2jVfap3nZ9x9XNXOQu1TA0VS7t9VNMZjlXKMPTB7ECQ8Iv0fPikbcTWf4O8r2X4eFfffv2voTDn39Or5lMa7AKEZAXpx937Bvm%2F9ielzoJJuUOPLZM3ovYmobbrTmAJ0Ws7sXxTMkV%2BeiHIHaao29p97Wyw4BX9nOosr%2FeL7ZqC%2BP0nuwLComVONtamR0pjxJx%2Bap2tKQ5rybRieks%2BvlZBfot0YIKbfQsqBK6UpbrwnK0RPf3TeIlStbf%2FPOHwQDI9kMt8u9%2FmTSP4i5VSVwJzDBlznnBDSre5on26FYCAM%2BgEd9ZiWmEd%2FjiZGlQvKuaQ%3D%3D)


| **Name**                | **Port(s)**  | **หน้าที่ (Role)**                                                                                                                                                                                                  | **จำนวน** |
| ----------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| **k3d-my-k8s-server-0** | -            | **Control Plane / Master Node:** เป็นเซิร์ฟเวอร์หลักของ Kubernetes Cluster ทำหน้าที่เป็น **API Server, Controller Manager, Scheduler** และมี **etcd** (ฐานข้อมูล) ในตัว                                             | **1 ตัว** |
| **k3d-my-k8s-agent-0**  | -            | **Worker Node:** เป็นโหนดทำงาน ทำหน้าที่รัน **Kubelet** และ **Kube-proxy** เพื่อจัดการและรัน Pod/Workloads ของคุณ                                                                                                   | **1 ตัว** |
| **k3d-my-k8s-agent-1**  | -            | **Worker Node:** เป็นโหนดทำงานตัวที่สอง (เหมือนตัวบน)                                                                                                                                                               | **1 ตัว** |
| **k3d-my-k8s-serverlb** | `39965:6443` | **Load Balancer (API Entrypoint):** เป็น Proxy/Load Balancer ที่อยู่ด้านหน้า Server Node(s) ทำหน้าที่เปิดพอร์ต **API Server** ออกมาภายนอก (ในที่นี้คือพอร์ต **39965** บนเครื่อง Host Map ไปยังพอร์ต **6443** ภายใน) | **1 ตัว** |
| **k3d-my-k8s-tools**    | -            | **Utility Container:** เป็น Container สำหรับเครื่องมือเสริมหรือใช้ในขั้นตอนเริ่มต้น/บำรุงรักษา ไม่ใช่ส่วนประกอบหลักของคลัสเตอร์ที่รันตลอดเวลา                                                                       | **1 ตัว** |


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/75466abef82790a8a8f86660b504e24d.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=poJZDKDNlj9bc9mAdFH4g4Me3JUIBn4KRuJyDti9x%2F2jXEqF0Qm%2FWTTlrMjzbBmY6ARx901gcMVoQYCdU5C4s9AzySYD1dCXp5iA%2B5tBTbtl%2Ft5IFRCaWxApRk7Xyq6kyflqJyggAxQKx2Y8UKqdNVEPg1F9Gsuqpa6Ks%2Bb5%2FpNZgQNoleqdDuDxtTkvzDdaK9DG%2BGkCToX9Uvqi9b%2BbEWceVLdwVMNCEezPjQZ0t%2FueMuBp8Vfdj2p2QPCFWV8PFUVXAP3GJpvivRWkclYsHRduITJ%2FJY9ghsI646BJcx9wkluQjMVeGTlRYC99SOJAXzboK%2BqC0VadsLUBcINflA%3D%3D)

- มี Master node = 1, worker node = 2
