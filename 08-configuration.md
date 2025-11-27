
# Configuration File


k8s configuration ทำได้โดยการเขียน yaml/json script


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/89c4e5b7e803966460b19da6ab2dd8ba.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=lQgKR7QTDHIjUfGK3PQkOMVYwoSMNPPvrgCvc7hiePCHlv6k0%2Bk7Sro7fHFejl2TLOLxLiHfNRysNieJeR4I72f3QkL9J30K8wWIr9Awpz5AjsAniu6z2Y1xEsuB6JvwyYGYVSI2zLaFmhGrLpHlvOxlrl1yPrn3foxUxRb576TvznEnv1pus3xpc3538pd6ZeYnop5y2YkHVBb8VGdFv6tKO%2BYQr5GoBzRI%2BF2vd%2BmVRH%2FTFxXphqKXDRXQct9h2CYb1VKurE1a84gA7w7QywQW5BKZK4B2U7ttvxjI7XsmxGx%2BbxEqEua%2Fzl%2FagDpPQe5Lfre2hJrc2Jwl5uW1mA%3D%3D)


สามารถแบ่งกลุ่มของ Configure ได้เป็น 3 ส่วนหลัก


## metadata:


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f56e1ee162858c0770082768bda3912e.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=DLJCQnRgG9v9CujCa32TX9t1KW6vj0vwhEY0ACZayn6fbUMgM8DiKbZfIWyR5LnwZsv7PxzlWYoz%2FusCRuHQ6SaBGUUbsW8yaEtbLrtzQayMiQ8J51I2%2BPII0xB3Bf%2Be7FQWkjdxmygv0nqfw3NAv9QFCxVjQkszWXLfM5NsFc9zcZgg5jOMAdQ4jRStBUZH3xTYWQcfvTn1HPH41KN6ZZ0U0uVrzRl3CPEu5EloH%2BnLMbt4EsGrY5QpO5sH8Ztbs3QHD5IKDPUll5%2FyuIJ7WeZmETn%2F34wQYKhZHk7sJhTymko%2Fw8woMcaQMTn9IbHnL0M%2BvF1ualTMhn2wSnASTw%3D%3D)

- สำหรับกำหนด
	- ชื่อของ object
	- namespace (จะนำไปไว้ที่ไหน)
	- labels (กำหนด Label ของ Application ทำให้ Service อื่นๆ filter ได้) เช่น filter เฉพาะ object ที่มี label: api

## spec:


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f1becbae1ebc19ce720219882941744c.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=QgaGs2NpjEQvLOKqZ2TXNg14K2sxzB6qw3hru%2F6MBUbnOa2tWCWlEn6UAnG7s4jD3WWCj8ozGdMNUnBdGfyAumA7Br8RiCG622F59Wh5g7DAdzk%2B5kapFISb%2FJ4kLjjV0KscAA68FhWmdsE0F4tjiHw0FjQDPT6d%2B4UKSPlDY4A1o5fT6r%2FzC6BgJ9SJeMtbpscKvtgj85mQbei3AjkUTW1OTTPx9Bu2ep%2FrQBIOK4egnVivm1GMw2gPThBA64cufc8ViFXMPpgG2xZxpI2OAUDhDGGGxICsSYA7hpSJPI%2FUTf3CcnLve3P946KeS4cm9lYAH%2BOZ322Am71z3bnMig%3D%3D)

- รายละเอียดของ object เช่น รายละเอียดของ object ที่ใช้ ซึ่งก็จะแตกต่างกันตาม object ที่กำหนดไว้ spec จะขึ้นอยู่กับส่วนของ object ที่จะกำหนด
	- image
	- spec
	- ports

## status:


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/050fa7e87a5de26fc4460e6dcab4c47b.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=eWZwj9pg%2BIvBi6ON3ioFMEsmERZoEYaqe7WSbalQ5i2o9VjJOCWrgQiTTr4wF%2Fj%2BopBnkwFovJOWnjJ8J0pPyiYuXV5LI%2BMbHOeFZxh9rpmL60k5j6KWTramc4Ax0YANu8%2Bs9I6uaZqGpjY44qLrCAZePoG7T3Wz589B0LCTA%2FdO07uU5Ra9eHgs0HLPCkId%2Fizvf38MePH6EiHpcNgjix6nDDSmdNiCQzzfmwqKi0rcAHV6frk8GDW%2BAQR5hDMYND23vUoaSNA3LsKHN0Km14Mayob70LTWv9IO21ymS%2BgJMhw%2F%2F2x1XJCLUkgS%2Fv2%2B2aoZDjs3TkyvHt5iWgW4Ww%3D%3D)

- เป็นส่วนของข้อมูลหลังจากการสร้าง Resource นั้นแล้ว เก็บ status ปัจจุบัน ซึ่งเก็บใน (ETCD) สามารถที่จะนำค่า information status ต่างๆ ออกมาดูได้
- K8s จะทำการเทียบ metadata กับ status ว่า status ตอนนี้มันตรงกับ metadata หรือไม่ ถ้าไม่ตรงสิ่งที่ K8s จะทำก็คือจะแก้ตัวของ object เพื่อให้สถานะ หรือ ข้อมูลตรงกัน เช่น Replica=3, status replica=2 ตัวของ ReplicaSet ก็จะต้อง start Pods ให้กลับมาเท่าเดิมใหม่

# Deployment Configuration


Container ที่เราจะติดตั้งลงไปจะผ่าน Deployment ก่อนโดยมีลักษณะการเขียน Configuration ดังนี้


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/9e5b33a646524ad0d00d1577f5978e57.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=oeuim0pceBl5inw5m4a5tnznLNoRPEZCfSFCwLBizHGkHGTyLBszLi%2BPFZdi1%2ByKSpAKifSwET1%2FeXxZ%2F6ZEVIrn%2FL889TGeoR5RjysRAPEqRcIDVqyJ3Jfg64dXHH%2FatsU2UVV%2BBtHRZduX34QKJilTMHEhrEZDINoWJgeWaZ56OYOScpHj8UBirhRj4FZH0CFZFF2RAt85eF4C7JiWENDxM1qGISzdvdZjPICvjq89pHE6f7LEyXTsyYjtit6e7kGuJEWaeT1iMKwip7T0nDxPVXwcQfDwaJZyn7GTCbqQatf8qlmQweUP6Z7LJxMD8ffh%2BzO7wfW9AvJUvJHPfQ%3D%3D)


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: nginx-deployment 
spec:
	selector:
		matchLabels:
			app: nginx
		template: # จะเห็นว่ามี metada และ spec ใต้ template
			metadata:
				labels:
					app: nginx
			spec:
				containers:
				- name: nginx
					image: nginx:1.14.2
```

- Flow: Deployment manage ReplicaSet —> ReplicaSet manage Pods —> Pods มี Containers อยู่ด้านใน
- ส่วน Highlight สีฟ้าด้านบนเป็น configure (Blueprint) ของ Deployment และด้านหลังสีแดงเป็น configure ของ Pods รายละเอียดที่จะสร้างแต่ละ Pods ขึ้นมาทุกตัวจะมี Specs เป็นยังไง
	- เช่น Start 3 Pods จะใช้ metadata เดียวกัน
		- nginx:1.14.2
		- labels: app: nginx
- selector ใช้บอกว่า Deployment นี้จัดการ Pods ไหนอยู่นั่นคือใช้ key “app: nginx” ในการ Matching ซึ่ง Deployment มันเป็น configure in configure (Deployment select Pods label)
- หาก key “matchLables → app” ไม่ตรงกับของ template มันจะไม่สามารถสร้าง Pods ได้เลยจะ errors

# Service Configuration


Component ที่เล็กที่สุดใน K8s คือ Object, Object ที่เล็กที่สุดคือ Pods, Pods ควรทำงานได้อย่าง Stable แล้ว Services รู้ได้ไงว่าจะ forward traffic จาก client ไปยัง Replicas ยังไง ซึ่งจะดูที่ key “app” เป็นหลัก


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/4e7be4a49b28bc9b9e7b83f157354477.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=PU8IvwVhnIhGKmWbh03u0zNfcmNJZEUDl2ejz91Dz4p1vDJYXqYEI%2BzdUs73i8fh8MP50udA%2F54x41ceFubD88EgEeRM7VBIkCJo9BUkj%2F4z46LAtLYtkfxsp8%2BiwLAgUskR%2FiXyAgk7TbRQHClTwt1FbmFEy%2FD8cjV5gakf81ddCrqcNV8HaGgGQI7tu7YSXRgH1ndn0jXCxtt54u5FbBUKvQmpTLnFV4YwT2uK7U2ow%2Baqmhg5uSMUQUMLY90X9g0mTJW3Poph3FVMUKLB%2Fm6deD%2F38q3DJAMP%2Ba5g8uw3tJnYO7HrlQCU4%2Ff%2BXQmM82qk8CYRgUvsllPB6TZIcA%3D%3D)

- จะเห็นว่าใน Deployment key “lables → app: my-web” โดย Service ก็จะ forward traffic ไปยัง Replicas โดยกำหนดผ่าน key “selector → app: my-web”

# Service - Port


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/8644c76cf908e87aa7ef362bae47035a.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=X8%2FX89B7k9vIxp1AIS%2B9KR0MEx3FSM02ONejovBD35iEzypn0ZF2bOAdPPdRm%2B55E2VmWLaPWU8JxRAyVk1yw8RBsZpVtHOD1UqvBtoKrc7Cgar7qPg9ls0vPcIDijetTYFCzGCVv44m3ZCAlW9IRlRe%2B7za5YFrNHhch9TSKFYnom5TokvSxpwvNpdRUi4%2Fe5vL6aKrJNzPnzf1LCGQy7mCsy%2FPBxhj1EFrzyX1VYJJjAHnjGJqt6mROLzJkogaiBSVVuBUX6AQ1D6wyb%2BoMqTkcUAvJBOH9xpZqNdQ6KjhTZNE2J%2Fp1%2FyDik5kbU65H2%2BR6ISTXBJTvqBlCsFpgg%3D%3D)

- Service จะ EXPOSE port 80 เปิดรับ connection ต่างๆ จากด้านนอกเข้ามาใน service แล้วจะ forward ไปที่ Target Pods ในที่นี้คือ key “targetPort: 80” โดย key นี้จะตรงกับ containerPort: 80 ที่เป็น Port ของ Pods

# Apply Configure File


```yaml
kubectl apply -f <config-file-name>
```


## ทดลองทำ


```yaml
$ kubectl apply -f deployment.yaml
```


### Get all object


```yaml
$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-77858f9f8f-q6z9d   1/1     Running   0          2m12s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   12h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   1/1     1            1           2m12s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-77858f9f8f   1         1         1       2m12s
```


### Describe deployment


```yaml
$ kubectl describe deployment nginx-deployment
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Wed, 26 Nov 2025 22:48:47 +0700
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:         nginx:1.14.2
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
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-77858f9f8f (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  2m32s  deployment-controller  Scaled up replica set nginx-deployment-77858f9f8f to 1
```


### Describe replicaset


```yaml
$ kubectl describe replicaset nginx-deployment-77858f9f8f
Name:           nginx-deployment-77858f9f8f
Namespace:      default
Selector:       app=nginx,pod-template-hash=77858f9f8f
Labels:         app=nginx
                pod-template-hash=77858f9f8f
Annotations:    deployment.kubernetes.io/desired-replicas: 1
                deployment.kubernetes.io/max-replicas: 2
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/nginx-deployment
Replicas:       1 current / 1 desired
Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
           pod-template-hash=77858f9f8f
  Containers:
   nginx:
    Image:         nginx:1.14.2
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age    From                   Message
  ----    ------            ----   ----                   -------
  Normal  SuccessfulCreate  3m30s  replicaset-controller  Created pod: nginx-deployment-77858f9f8f-q6z9d
```


### Describe pod


```yaml
$ kubectl describe pod nginx-deployment-77858f9f8f-q6z9d
Name:             nginx-deployment-77858f9f8f-q6z9d
Namespace:        default
Priority:         0
Service Account:  default
Node:             k3d-my-k3d-server-0/172.18.0.3
Start Time:       Wed, 26 Nov 2025 22:48:47 +0700
Labels:           app=nginx
                  pod-template-hash=77858f9f8f
Annotations:      <none>
Status:           Running
IP:               10.42.3.9
IPs:
  IP:           10.42.3.9
Controlled By:  ReplicaSet/nginx-deployment-77858f9f8f
Containers:
  nginx:
    Container ID:   containerd://85f517668edee3b1853b42dff7485e92c2ae5b9a020f8e51b21c54473b5da84f
    Image:          nginx:1.14.2
    Image ID:       docker.io/library/nginx@sha256:f7988fb6c02e0ce69257d9bd9cf37ae20a60f1df7563c3a2a6abe24160306b8d
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 26 Nov 2025 22:48:48 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-bwkd5 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-bwkd5:
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
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  3m57s  default-scheduler  Successfully assigned default/nginx-deployment-77858f9f8f-q6z9d to k3d-my-k3d-server-0
  Normal  Pulled     3m57s  kubelet            Container image "nginx:1.14.2" already present on machine
  Normal  Created    3m57s  kubelet            Created container nginx
  Normal  Started    3m57s  kubelet            Started container nginx
```


### Destroy


```yaml
$ kubectl delete -f deployment.yaml
deployment.apps "nginx-deployment" deleted from default namespace
```

