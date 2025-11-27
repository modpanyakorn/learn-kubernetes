
# LAB


ถ้าเกิดว่าเราจะจำลอง Environment เรามี Cluster 1 Cluster

- Cluster non-production
- ต้องการแบ่งพื้นที่ให้ Dev, QC
- แยกให้ชัดระหว่าง Application information, Infrastructure information
- เราจะแบ่ง Environment 2 ตัวคือ
	- Dev
	- UAT

## 1. Create Namespace

- dev-environment
- uat-environment

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-environment
  labels:
    name: dev-environment
---
apiVersion: v1
kind: Namespace
metadata:
  name: uat-environment
  labels:
    name: uat-environment
```


## 2. Apply Dev Resource


```yaml
# simple-app.1.0.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-deployment
  labels:
    app: my-web-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-web
  template:
    metadata:
      labels:
        app: my-web
    spec:
      containers:
      - image: nginx
        name: my-web-on-nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: my-web-service
spec:
  selector:
    app: my-web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

- โดยเราจะ apply เข้าไปใน namespace=dev-environment โดยใช้คำสั่ง

	```bash
	$ kubectl apply -f simple-app.1.0.yaml -n dev-environment
	deployment.apps/my-web-deployment created
	service/my-web-service created
	```

- show resource dev-environment namespace

	```bash
	$ kubectl get all -n dev-environment
	NAME                                     READY   STATUS    RESTARTS   AGE
	pod/my-web-deployment-799cd6f867-7kdzq   1/1     Running   0          51s
	pod/my-web-deployment-799cd6f867-ljx5t   1/1     Running   0          51s
	
	NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
	service/my-web-service   ClusterIP   10.43.106.195   <none>        80/TCP    51s
	
	NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
	deployment.apps/my-web-deployment   2/2     2            2           51s
	
	NAME                                           DESIRED   CURRENT   READY   AGE
	replicaset.apps/my-web-deployment-799cd6f867   2         2         2       51s
	```


## 3. Apply UAT Resource


Scenario คือ หลังจากที่เรา Dev เสร็จแล้วอยาก test on UAT เราก็จะ apply configure ที่เหมือนกับ Dev เข้าไปใน UAT เหมือนกัน


```bash
$ kubectl get all -o wide -A | grep -E "dev-environment|uat-environment"
# pod
dev-environment   pod/my-web-deployment-799cd6f867-7kdzq        1/1     Running     0          5m47s   10.42.1.6   k3d-my-cluster-agent-2    <none>           <none>
dev-environment   pod/my-web-deployment-799cd6f867-ljx5t        1/1     Running     0          5m47s   10.42.3.7   k3d-my-cluster-agent-1    <none>           <none>
uat-environment   pod/my-web-deployment-799cd6f867-wswjq        1/1     Running     0          98s     10.42.1.7   k3d-my-cluster-agent-2    <none>           <none>
uat-environment   pod/my-web-deployment-799cd6f867-x7ssx        1/1     Running     0          98s     10.42.4.4   k3d-my-cluster-agent-0    <none>           <none>

# service
dev-environment   service/my-web-service   ClusterIP      10.43.106.195   <none>                                        80/TCP                       5m47s   app=my-web
uat-environment   service/my-web-service   ClusterIP      10.43.172.174   <none>                                        80/TCP                       98s     app=my-web

# deployment
dev-environment   deployment.apps/my-web-deployment        2/2     2            2           5m47s   my-web-on-nginx          nginx                                      app=my-web
uat-environment   deployment.apps/my-web-deployment        2/2     2            2           98s     my-web-on-nginx          nginx                                      app=my-web

# replicaset
dev-environment   replicaset.apps/my-web-deployment-799cd6f867        2         2         2       5m47s   my-web-on-nginx          nginx                                      app=my-web,pod-template-hash=799cd6f867
uat-environment   replicaset.apps/my-web-deployment-799cd6f867        2         2         2       98s     my-web-on-nginx          nginx                                      app=my-web,pod-template-hash=799cd6f867
```


## 4. Access Resource


เราจะ Access เข้าไปยังไงถ้าทั้ง 2 Environment กำลังทำงานอยู่ และอยากเข้าไปใน Resource ที่แตกต่างกัน

- ถ้าหาก Application ใช้ Container Port Configure เดียวกันอยู่ การใช้ NodePort จะทำไม่ได้เพราะ Port จะชนกัน (Node Port จะ Expose Port นั้นที่ Node IP Address)
- ซึ่งเราจะใช้ Ingress (Forward External to Internal) และ Mapping Path ตาม Service นั้นๆ ที่ต้องการแยกกันระหว่าง Dev, UAT

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-web-ingress
  namespace: dev-environment # ระบุ namespace dev-environment
spec:
  rules:
  - host: "dev.my-env.com" # ถ้า client request ผ่าน url นี้
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-web-service
            port:
              number: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-web-ingress
  namespace: uat-environment # ระบุ namespace uat-environment
spec:
  rules:
  - host: "uat.my-env.com" # ถ้า client request ผ่าน url นี้
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-web-service
            port:
              number: 80
---
```

- เราแยก namespace โดยการใช้ key “namespace”
- apply ingress

## 5. Configure Hosts file


เราต้องทำ DNS ที่ 

- linux: /etc/hosts
- windows: C:\Windows\System32\drivers\etc\hosts

เพื่อที่จะ resolve ip จาก 127.0.0.1 เป็น url [dev.my-env.com](http://dev.my-env.com/) และ [uat.my-env.com](http://uat.my-env.com/) 


```yaml
# k8s resolve hosts
127.0.0.1   dev.my-env.com
127.0.0.1   uat.my-env.com
```


## 6. Update App Version

- เราจะเปลี่ยน image จาก nginx เป็น httpd ใน version ใหม่

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-deployment
  labels:
    app: my-web-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-web
  template:
    metadata:
      labels:
        app: my-web
    spec:
      containers:
      - image: httpd
        name: my-web-on-nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: my-web-service
spec:
  selector:
    app: my-web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

- apply `kubectl apply -f simple-app.2.0.yaml --namespace=dev-environment`

	```bash
	$ kubectl get all -n dev-environment
	NAME                                     READY   STATUS    RESTARTS   AGE
	pod/my-web-deployment-76b7574555-8fbcp   1/1     Running   0          56s
	pod/my-web-deployment-76b7574555-8xcjz   1/1     Running   0          72s
	
	NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
	service/my-web-service   ClusterIP   10.43.106.195   <none>        80/TCP    44m
	
	NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
	deployment.apps/my-web-deployment   2/2     2            2           44m
	
	NAME                                           DESIRED   CURRENT   READY   AGE
	replicaset.apps/my-web-deployment-76b7574555   2         2         2       72s
	replicaset.apps/my-web-deployment-799cd6f867   0         0         0       44m
	```

	- ตัวของ Replica ตัวใหม่จะมี Pod ขนมาที่ตัวใหม่ และจะลบตัวเก่าออก

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b2c5e7f0c54b66111632f9c710e6c4ab.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=VT6HotM%2BV2HlJzqRJmM%2BV7tD0jTWgluDe5l%2FEyYfUjWKKnRfaLEC%2BtMV8eUYSyForP3cKeIcz7wjnlma3nLL8UPjUJVk4xcBlhfI5Se8NUfYmFJb4Ez0mes4wQrnsf7l0%2BdPHERflGPfdlCvzMSAlRU5gcMk7IFQsHlE5Ri%2Fu91sAY59AL95XbefrKE3KOoYkI0uSvJcb0urDR%2FSpBNuoGKpED4lbzGPvwQotZvbKEzznWVI2jJ6dDp7WuIqalG3ZDsmqF%2F8kJbs3tEEC0rZiJ0HfWO%2BMwp%2FkU%2B7iZz3aPH3uDg0vmz4sW4N5iOlBePsYQkgZmmop0WpOYb6FObhiA%3D%3D)


## Info

- dev-environment

	```bash
	$ kubectl get all -n dev-environment
	NAME                                     READY   STATUS    RESTARTS   AGE
	pod/my-web-deployment-76b7574555-8fbcp   1/1     Running   0          3m19s
	pod/my-web-deployment-76b7574555-8xcjz   1/1     Running   0          3m35s
	
	NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
	service/my-web-service   ClusterIP   10.43.106.195   <none>        80/TCP    46m
	
	NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
	deployment.apps/my-web-deployment   2/2     2            2           46m
	
	NAME                                           DESIRED   CURRENT   READY   AGE
	replicaset.apps/my-web-deployment-76b7574555   2         2         2       3m35s
	replicaset.apps/my-web-deployment-799cd6f867   0         0         0       46m
	```

- uat-environment

	```bash
	$ kubectl get all -n uat-environment
	NAME                                     READY   STATUS    RESTARTS   AGE
	pod/my-web-deployment-799cd6f867-wswjq   1/1     Running   0          43m
	pod/my-web-deployment-799cd6f867-x7ssx   1/1     Running   0          43m
	
	NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
	service/my-web-service   ClusterIP   10.43.172.174   <none>        80/TCP    43m
	
	NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
	deployment.apps/my-web-deployment   2/2     2            2           43m
	
	NAME                                           DESIRED   CURRENT   READY   AGE
	replicaset.apps/my-web-deployment-799cd6f867   2         2         2       43m
	```


## 7. Delete Namespace


```bash
$ kubectl delete -f diff-2-environment.yaml 
namespace "dev-environment" deleted
namespace "uat-environment" deleted
```

