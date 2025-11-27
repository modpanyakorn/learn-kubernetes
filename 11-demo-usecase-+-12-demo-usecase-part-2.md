
# Deployment


## Deploy MariaDB


```yaml
# mariadb.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mariadb-deployment
spec:
  selector:
    matchLabels:
      app: mariadb-app
  template:
    metadata:
      labels:
        app: mariadb-app
    spec:
      containers:
      - name: mariadb-container
        image: mariadb:12.2.1-rc
        ports:
        - containerPort: 3306
---
apiVersion: v1
kind: Service
metadata:
  name: database-service
spec:
  selector:
    app: mariadb-app
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 3306

```

1. ใส่ส่วนของ image, label name, port ต่างๆ ซึ่งก็จะมีสิ่งที่เราต้อง Concern คือเรื่องของ Service Name, Port ถ้าเราไม่อยากให้ User รู้ว่าเราใช้ Stack หรือ Port อะไรก็ควรจะใช้ชื่ออื่น และ External Port ที่ไม่ตรงกับ Internal Port
2. เพิ่มส่วนของ env เนื่องจาก mariadb image requried 
`MARIADB_ROOT_PASSWORD=my-secret-pw`

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/dcd705cce8fabb528277a10cec38e6db.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=kUCW%2B27LhtlCgrxN4EDWONagTIV9Pq%2BDk3HmyGFyWDWHOm0o%2FNOlq7NsM3Qz57PFL6sKcLl2%2BXg%2F0PIri1SJ%2BTQAXlbowRwegHL9ExCD4M63qEAPXytV7HgYdDowX4znNowqqGDF3FVXumeF0lJVaHPjS8vWH4jwTmyI%2Fxeffuwb4r7994vVXPrAKgsjrUL7vaJ2ulUn7kx4CtOlAStAjx09E6E3XKZ6HU5XF2a96icte%2BRw5QBRthiQIro1psBKhokoH2d9cUCqwR67QzqSsfwfnsuGRMGWgU882MIgq25S4UQJgHOJSHk5tw9sws6vof0BkeHZ7saDrzSuQt1Uzg%3D%3D)


## Configure Environment Variable with Secret


เราจะใช้ Secret ในการเก็บข้อมูลแทนการ Hard Configure ลงใน Script และจะ Encoding Secret ไว้ด้วย


ต้องสร้าง Secret ก่อน

1. Encoding base64: 1234 แล้วใส่ใน script จากนั้น Apply สร้าง Secret

	```yaml
	apiVersion: v1
	kind: Secret
	metadata:
	  name: database-secret
	type: Opaque
	data:
	  db-root-user: MTIzNA==
	```


	```bash
	$ kubectl get secret
	NAME              TYPE     DATA   AGE
	database-secret   Opaque   1      33s
	```

2. เปลี่ยน key “value” —> valueFrom: secretKeyRef:” ใน Deployment script

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/c7e686fac0499a6a306797aacda69988.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=VqIJ%2B1wKWbPZz6QI9429S41B%2B8O2681a4B1PPYR2ksc%2BH4xkYDNbXW1DBUa%2FhUzWpfr8mL1p6Y03%2Fkj2KfTYDe%2FAWd4KR9U0SHKTEqpDGs9ZIl7eIX1YCgiESolVRcoklKJT4Etlippc6BS2XCZeTMuYVwK7t6c8t3oTiiNvs%2FwXedt6cD8UV6ijg0iTdTa9HNxPhjbXYHSppxmxWzvQyvBFY1Eq2r6kbPRjwdCUvv0Uk8dxES8upyBZ6tt%2Fyn4VbYGXr%2FExposQ1F%2F4YIwyGiXd0ngNsRwA%2FxzniAwe2YJcXIH7pM5UNjzlRHROSKohtKSB5SHxBkY%2BOJ9yAnc4aQ%3D%3D)

- สร้าง Environment Variable เพิ่มเติม

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/3d5e1551ecd2450866aa20677440f027.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=bFIHCX6NWvBSjcqad1%2FPowyz5B9uu7InedX1jNYPNT5dI78a%2Fka3w1DsoB%2B6DxGaXUvZidSf%2BENZjT%2FsW2zsZwAeG2pLUmxt9m06vwfbSsIUMkD8TrUD0MbxrHndNz1wdgNljHppVxgh9iBCYpdbpgCFoCdPAjOTnatWOxlUF0kMAlcrX01xW4TiyK38K8YcNX9CZ1YUx52%2FpdVJGx8GxAJXHOMI7DUUH%2Fmrzxlvv6o5Ferl8LIO0mqqtZjUh3HwREQsKGAFK72N1Sk6NEHwMIFhc4Zh2EF02LrQoi6SjXbMEz3DpU%2B4m%2BD1piQ4xR7OYVMOG6yyXC5upuMo5Qb%2FdA%3D%3D)


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/6334b6c9c9ab59607ef8a0b33df16a99.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=fSwA4XVIIMJUQ3cfJOvomfkgOvcCpt602p7pwHQpiQ5MYX5RcXwlMdABHB2JHmqCynQT5lFZ049UfXR1VPxrPt%2F7S1eYLLa5KjgHVS%2BqdyoXZ9ux6b7WzUZZVuOwEZxoVb5MVTkBPe3yMqNr4CKf0RHEOVew%2FupENnLgImuhdlW7Eq%2B3W7XEUcsDQwtJhUu6brMcXvGa3jMjOyCTn%2BDmyF1rHpkCth0LiNjhJX5nytXNftdOM%2BHnibUYY70GEXVDSktPKjz9TJ%2FmpFnA6kXIVFsDffPziAtZZaB2B6DtPEvLT%2BWZtnXQZ7snrHMvBt%2FkrWzfJz4gapZowiYR82SYjA%3D%3D)

1. apply `mariadb.yaml` อีกครั้ง

	```yaml
	$ kubectl describe pod
	Name:             mariadb-deployment-6ffd9b795-l2489
	Namespace:        default
	Priority:         0
	Service Account:  default
	Node:             k3d-my-cluster-agent-2/172.18.0.4
	Start Time:       Thu, 27 Nov 2025 12:59:32 +0700
	Labels:           app=mariadb-app
	                  pod-template-hash=6ffd9b795
	Annotations:      <none>
	Status:           Running
	IP:               10.42.1.4
	IPs:
	  IP:           10.42.1.4
	Controlled By:  ReplicaSet/mariadb-deployment-6ffd9b795
	Containers:
	  mariadb-container:
	    Container ID:   containerd://ff91c98ece2ebd52e6573f9ebc8dfa1fd083eadebcf0968066495a3db707b4cb
	    Image:          mariadb:12.2.1-rc
	    Image ID:       docker.io/library/mariadb@sha256:f6914ac85385903b2e67a4cb34cfbc6789f49fd40c640f41a15436e1d63758e3
	    Port:           3306/TCP
	    Host Port:      0/TCP
	    State:          Running
	      Started:      Thu, 27 Nov 2025 12:59:56 +0700
	    Ready:          True
	    Restart Count:  0
	    Environment:
	      MYSQL_ROOT_PASSWORD:  <set to the key 'db-root-password' in secret 'database-secret'>  Optional: false
	      MARIADB_USER:         <set to the key 'db-default-user' in secret 'database-secret'>   Optional: false
	      MARIADB_PASSWORD:     <set to the key 'db-password' in secret 'database-secret'>       Optional: false
	    Mounts:
	      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-tdxlg (ro)
	Conditions:
	  Type                        Status
	  PodReadyToStartContainers   True
	  Initialized                 True
	  Ready                       True
	  ContainersReady             True
	  PodScheduled                True
	Volumes:
	  kube-api-access-tdxlg:
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
	  Normal  Scheduled  2m46s  default-scheduler  Successfully assigned default/mariadb-deployment-6ffd9b795-l2489 to k3d-my-cluster-agent-2
	  Normal  Pulling    2m45s  kubelet            Pulling image "mariadb:12.2.1-rc"
	  Normal  Pulled     2m22s  kubelet            Successfully pulled image "mariadb:12.2.1-rc" in 23.187s (23.187s including waiting). Image size: 106765907 bytes.
	  Normal  Created    2m22s  kubelet            Created container mariadb-container
	  Normal  Started    2m22s  kubelet            Started container mariadb-container
	```

- จะเห็นว่าใน Environment ก็จะมีการ Parsing Environment Variable เข้ามาใช้ใน Container และ k8s จะทำการ Encoding ให้

	```bash
	root@mariadb-deployment-6ffd9b795-l2489:/# env | grep "MYSQL*"
	MYSQL_ROOT_PASSWORD=1234
	root@mariadb-deployment-6ffd9b795-l2489:/# env | grep "MARIA*"
	MARIADB_USER=user
	MARIADB_VERSION=1:12.2.1+maria~ubu2404
	MARIADB_PASSWORD=1234
	```


## Deploy adminer


เราจะใช้ adminer ในการ manage MariaDB เป็น GUI Service

1. เพิ่ม adminer configure

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: adminer-deployment
spec:
  selector:
    matchLabels:
      app: adminer-app
  template:
    metadata:
      labels:
        app: adminer-app
    spec:
      containers:
      - name: adminer-container
        image: adminer:5.4.1-standalone
        ports:
          - containerPort: 8080
        env:
          - name: ADMINER_DEFAULT_SERVER
            valueFrom:
              configMapKeyRef:
                name: database-config
                key: db_url
---
apiVersion: v1
kind: Service
metadata:
  name: adminer-service
spec:
  selector:
    app: adminer-app
  ports:
    - protocol: TCP
      port: 8999
      targetPort: 8080

```

- apply `adminer.yaml`
1. เพิ่ม DBSERVER ให้ Adminer Monitoring
- ใช้ ConfigMap กำหนด Key:Value

	```yaml
	apiVersion: v1
	kind: ConfigMap
	metadata:
	  name: database-config
	data:
	  db_url: database-service:5000
	```

	- เนื่องจากตัวของ Adminer รู้จัก port มาตรฐานของ DB เป็น 3306 และ service ของเราไม่ได้ใช้ 3306 แต่ใช้ 5000 จึงต้องระบุ port 5000 ไว้
- apply `configmap.yaml`

	```bash
	$ kubectl describe configmap database-config
	Name:         database-config
	Namespace:    default
	Labels:       <none>
	Annotations:  <none>
	
	Data
	====
	db_url:
	----
	database-service
	
	
	BinaryData
	====
	
	Events:  <none>
	```

- จากนั้นแก้ Adminer Script

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/7dfff205af4869b892788ac00f2a0eba.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=fvTdPGpJ1f04bugeUW9%2FfA6ts9y8yWiZCyVl08829eVChmJEVX4SuCTF76pEvlmCm1xsSG3%2FvYI1PiHt5HN5Xhm0Js9xtmk8UBkY9vz3vyZbne3oVfhOYD2zXbk8RkRbTHemBS1Qr4Ov%2FxApFUW9L4IH1BLuGD3lmTHFMADvoYs%2FWakSYy7iilnzfZOHBxSDmQA0QviKTzTFeWZrRQxVITwQ1J6WQ67PjgIsUOrAEvRooaRA%2B10cq60CZPILWaNrG%2BCKgL4M8f4fHQjJ2KLNEV0YrkFQiQW6l522Ix9WPlG70YkCqlIjONYnNTpoOGYjG1UV2bho9yPfzyRqasS%2BbA%3D%3D)

- และ apply `adminer.yaml`

	```bash
	$ kubectl get all
	NAME                                      READY   STATUS              RESTARTS   AGE
	pod/adminer-deployment-66974c8d88-d9k8z   1/1     Running             0          12m
	pod/adminer-deployment-8646ff565f-n75nd   0/1     ContainerCreating   0          9s
	pod/mariadb-deployment-6ffd9b795-l2489    1/1     Running             0          38m
	
	$ kubectl get all
	NAME                                      READY   STATUS    RESTARTS   AGE
	pod/adminer-deployment-8646ff565f-n75nd   1/1     Running   0          23s
	pod/mariadb-deployment-6ffd9b795-l2489    1/1     Running   0          38m
	```

	- จะเห็นว่ามีการสร้าง Pod ขึ้นมาใหม่ เมื่อ Pod ใหม่ Running แล้ว ก็จะลบ Pod เดิมออกไป

## Setup Ingress


เราจะ mapping path ไปที่ adminer-service (MariaDB)

1. เพิ่ม service: name, port: number ของ database-service เข้าไป

```yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: adminer-ingress
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: adminer-service
            port:
              number: 8999
```

