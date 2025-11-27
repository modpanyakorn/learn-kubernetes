
# Create Deployment object

- เมื่อเราสร้าง Deployment, ReplicaSet จพสร้างขึ้นมาโดยอัติโนมัติ ReplicaSet มีหน้าที่ดูแลให้มีจำนวน Pods ที่ทำงานได้อย่างถูกต้อง ตามจำนวนที่กำหนดไว้ใน Deployment เสมอ

```bash
$ kubectl create deployment nginx --image=nginx
deployment.apps/nginx created

$ kubectl get pod
NAME                     READY   STATUS    RESTARTS   AGE
nginx-676b6c5bbc-jm5kd   1/1     Running   0          5m3s
```

- ถ้าเราส้ราง Pod ขึ้นมาเองจะเป็นชื่อตามที่เราส้ราง เมื่อสร้างผ่าน Deployment, จะสร้าง Pod และมีชื่อเป็น random name

ดู object ทั้งหมดของเรา


# Show all objects


```bash
$ kubectl get all
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-676b6c5bbc-jm5kd   1/1     Running   0          6m1s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   9h # เป็น Default Service ของ K8s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           6m1s # Deployment Object

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-676b6c5bbc   1         1         1       6m1s # ReplicaSet
```

- สังเกตว่าถ้าเราสร้าง Deployment —> Create ReplicaSet —> Control Pods เราจะเห็นว่า unique number “676b6c5bbc” ที่เอาไว้อ้างอิง ReplicaSet กับ Pod และ “jm5kd” คือ ID ของ Pod
- Flow: เราสร้าง Deployment แล้ว Deployment จะไปสร้าง ReplicaSet แล้ว gen unique id มาชุดนึงคือ “676b6c5bbc” เสร็จแล้ว ReplicaSet จะไปสร้าง Pod ตาม Configuration ของเราแล้วสร้าง unique id อีกทีคือ “jm5kd”

# Describe Pods


```bash
$ kubectl describe pod nginx-676b6c5bbc-jm5kd
Name:             nginx-676b6c5bbc-jm5kd
Namespace:        default
Priority:         0
Service Account:  default
Node:             k3d-my-k3d-server-0/172.18.0.3
Start Time:       Wed, 26 Nov 2025 19:52:05 +0700
Labels:           app=nginx
                  pod-template-hash=676b6c5bbc
Annotations:      <none>
Status:           Running
IP:               10.42.3.5
IPs:
  IP:           10.42.3.5
Controlled By:  ReplicaSet/nginx-676b6c5bbc
Containers:
  nginx:
    Container ID:   containerd://5aa3d7f91d394854b802db4a15389755d7e9ddab233495a1b498aa034783d448
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:553f64aecdc31b5bf944521731cd70e35da4faed96b2b7548a3d8e2598c52a42
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 26 Nov 2025 19:52:08 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-n9rs9 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-n9rs9:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  31m   default-scheduler  Successfully assigned default/nginx-676b6c5bbc-jm5kd to k3d-my-k3d-server-0
  Normal  Pulling    31m   kubelet            Pulling image "nginx"
  Normal  Pulled     31m   kubelet            Successfully pulled image "nginx" in 1.868s (1.868s including waiting). Image size: 59772801 bytes.
  Normal  Created    31m   kubelet            Created container nginx
  Normal  Started    31m   kubelet            Started container nginx
```

- จะเห็นว่า Pod นี้ถูกควยคุมด้วย ReplicaSet/nginx-676b6c5bbc
- ดู ReplicaSet

# Show ReplicaSet


```bash
$ kubectl get replicaset
NAME               DESIRED   CURRENT   READY   AGE
nginx-676b6c5bbc   1         1         1       46m

$ kubectl describe replicaset nginx-676b6c5bbc
Name:           nginx-676b6c5bbc
Namespace:      default
Selector:       app=nginx,pod-template-hash=676b6c5bbc
Labels:         app=nginx
                pod-template-hash=676b6c5bbc
Annotations:    deployment.kubernetes.io/desired-replicas: 1
                deployment.kubernetes.io/max-replicas: 2
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/nginx
Replicas:       1 current / 1 desired
Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
           pod-template-hash=676b6c5bbc
  Containers:
   nginx:
    Image:         nginx
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  46m   replicaset-controller  Created pod: nginx-676b6c5bbc-jm5kd
```

- Replicas: 1 current / 1 desired  จะเห็นว่าตัวของ Pod ที่ต้องการมี 1 แล้ว Pod ยัง Running อยู่ 1 Pod แสดงว่ายังตรงกับ desired อยู่
- Controlled By:  Deployment/nginx จะเห็นว่า ReplicaSet จะถูก control ด้วย Deployment:ที่ชื่อ “nginx” ที่เราสร้างไว้ซึ่งจะตรงตาม Flow ที่ได้เขียนไว้

# Change Pods Image


เราสามารถเปลี่ยน image version ของ container โดยใช้


```bash
$ kubectl edit deployment nginx
```


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/0ac9dfa7a11eb3f489fedac53c243be3.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=R7BAgcAbBqOQmuq3exa2uZK%2FgQmPcyxIk3ursSY5Bdj2tiL8rlFw5uyCqTnfp%2FbL03Gi6DhO%2BMNamKnSjSurQvdVk7lWDrO5LO9ofvjxcVxyaNaJfCM17EhqU0CinMghMIzyiwsj%2BCGb7Mh27K7nHR7J47NQ361oI4UUprFYw6GgY88ttAT2fe0S8oioaxUD9St7bt%2FJFMcHR%2B78oC4r%2BHcDb9D8GZOJC3WGUFKQc3uFm8Bn13c38cxSI1AgNOvkmtzCS9uqD%2Fz0%2Brsmy6mBbz%2Fz90XJooqx7i0jz0CQtf1i%2BA4a0UbHm6YjnNTx%2FOlJVUuy6quKWpKPDS3NRzeI0w%3D%3D)

- เราได้เปลี่ยน image version ใหม่จากเดิม

```bash
$ kubectl get all
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-7b847b8c4c-rsnjp   1/1     Running   0          25s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   10h

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           56m

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-676b6c5bbc   0         0         0       58m
replicaset.apps/nginx-7b847b8c4c   1         1         1       25s
```

- จะเห็นว่าตัวของ Pod จะมี AGE ที่พึ่งทำการ Start ใหม่ขึ้นมา
- และ ReplicaSet ตัวใหม่ขึ้นมาทำงานและลบตัวเก่าออกสังเกตจาก unique id: nginx-676b6c5bbc —> nginx-7b847b8c4c
- ซึ่งการเปลี่ยนแปลงคือสร้าง ReplicaSet ขึ้นมาใหม่และสร้าง Pods เข้าไปใน ReplicaSet (ถ้าตัวใหม่สร้างเรียบร้อยทำานได้ถูกต้อง และลบตัวเก่าทิ้งไป)

# Describe Deployment


```bash
$ kubectl describe deployment nginx
Name:                   nginx
Namespace:              default
CreationTimestamp:      Wed, 26 Nov 2025 19:52:05 +0700
Labels:                 app=nginx
Annotations:            deployment.kubernetes.io/revision: 2
Selector:               app=nginx
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:         nginx:1.29-trixie-perl
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  nginx-676b6c5bbc (0/0 replicas created)
NewReplicaSet:   nginx-7b847b8c4c (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  18m   deployment-controller  Scaled up replica set nginx-7b847b8c4c to 1
  Normal  ScalingReplicaSet  18m   deployment-controller  Scaled down replica set nginx-676b6c5bbc to 0 from 1
```

- จะเห็นว่าจาก Event ตัวของ ReplicaSet: replica set nginx-7b847b8c4c to 1 ได้ถูกสร้างขึ้น และลบตัวของ nginx-676b6c5bbc to 0 from 1 ออกไป

# Add more Pods


ใช้คำสั่ง `kubectl edit deployment nginx` และแก้ที่ key replicas


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f623d2453feef213d6c077a271dd4621.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=Z%2FI4OAVGv%2BLpdvASiFmbOFsTn8ierYcJtoSsBF18sIfxFvHoWMrybifDIGmzjLqVTjiPZ9Ofw2qq3opXMMYIwC0oWb%2BmLSFfm49DoaSElZR5BjiTC9hrE1jK5JpV86%2B40ptxq8QdfC6alojWez%2Fsu60ZDMe6Qafsrdmhb0EMaAJK0VS9OeLZngFiWZboPD4LEet5fzLhYGOaBiBMkH7HZdnUFYO9DolKpfDtdfhpzkyFSt2nHUqlc8XPiOnbu5elmT7jqaOc5wvti2sx6QMRSHq2FtDY2ChcZ2WSHGpLXSiSoDt2qIRENJNUz1Wo2KNMWHgOExGW7mJ71b7vFVEWww%3D%3D)


```bash
$ kubectl get pod
NAME                     READY   STATUS              RESTARTS   AGE
nginx-7b847b8c4c-r2hd8   0/1     ContainerCreating   0          5s
nginx-7b847b8c4c-rsnjp   1/1     Running             0          22m
nginx-7b847b8c4c-v8s6f   0/1     ContainerCreating   0          5s
```

- ก็จะเห็นว่ามี Pod ใหม่ถูกสร้างขึ้นมา เราลอง get all และ describe ดู

```bash
$ kubectl get all
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-7b847b8c4c-r2hd8   1/1     Running   0          61s
pod/nginx-7b847b8c4c-rsnjp   1/1     Running   0          23m
pod/nginx-7b847b8c4c-v8s6f   1/1     Running   0          61s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   10h

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   3/3     3            3           79m

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-676b6c5bbc   0         0         0       79m
replicaset.apps/nginx-7b847b8c4c   3         3         3       23m
```

- ก็จะมี pod เพิ่มขึ้นมาตามความต้องการ (DESIRED) คือ 3 Pods ตาม Replicas

```bash

$ kubectl describe nginx-7b847b8c4c
error: the server doesn't have a resource type "nginx-7b847b8c4c"
panyakorn@Panyakorn-mod:~$ kubectl describe replicaset nginx-7b847b8c4c
Name:           nginx-7b847b8c4c
Namespace:      default
Selector:       app=nginx,pod-template-hash=7b847b8c4c
Labels:         app=nginx
                pod-template-hash=7b847b8c4c
Annotations:    deployment.kubernetes.io/desired-replicas: 3
                deployment.kubernetes.io/max-replicas: 4
                deployment.kubernetes.io/revision: 2
Controlled By:  Deployment/nginx
Replicas:       3 current / 3 desired
Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
           pod-template-hash=7b847b8c4c
  Containers:
   nginx:
    Image:         nginx:1.29-trixie-perl
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  23m   replicaset-controller  Created pod: nginx-7b847b8c4c-rsnjp
  Normal  SuccessfulCreate  76s   replicaset-controller  Created pod: nginx-7b847b8c4c-v8s6f
  Normal  SuccessfulCreate  76s   replicaset-controller  Created pod: nginx-7b847b8c4c-r2hd8
```

- และมี Events บอกว่ามีการสร้าง Pods เพิ่ม 2 Pods เมื่อ 76s ที่แล้วขึ้นมาใน ReplicaSet: nginx-7b847b8c4c
- เราไม่ได้สร้าง Pods โดยตรงเราปรับจำนวนของ Replicas ซึ่งตัวของ ReplicaSet จะตรวจสอบ Cluster อยู่เสมอถ้า Pods มีน้อยกว่าจำนวน Replicas ที่กำหนด ReplicaSet จะสร้าง Pods มาใหม่จนครบจำนวนที่กำหนด
- การเปลี่ยนแปลง Configure ด้วย kubectl edit ไม่ใช่วิธีที่ดีเป็นวิธี Hard Code ควรมี script file ในการแก้ หรือใช้คำสั่งในการ output configure yaml file ออกมาด้วย `$ kubectl get deployment nginx -o yaml`

# Configure Replicas by `scale` command


สมมุติว่าต้องการปรับ Replicas ให้เป็น 5 ใช้คำสั่ง `kubectl scale deployment/nginx --replicas=5` 


```bash
$ kubectl get pod
NAME                     READY   STATUS              RESTARTS   AGE
nginx-7b847b8c4c-blqq4   0/1     ContainerCreating   0          14s
nginx-7b847b8c4c-r2hd8   1/1     Running             0          17m
nginx-7b847b8c4c-r6vck   1/1     Running             0          14s
nginx-7b847b8c4c-rsnjp   1/1     Running             0          39m
nginx-7b847b8c4c-v8s6f   1/1     Running             0          17m
```

- ซึ่งเราสามารถปรับ Scale ได้ผ่านคำสั่ง `scale` หรือ edit โดยตรงผ่านคำสั่ง `edit`
