
# Service - Stable IP


เป็น IP เสมือน (Virtual IP) ที่ถูกสร้างขึ้นภายในคลัสเตอร์ (Cluster IP) IP นี้จะไม่เปลี่ยนแปลงตราบใดที่ Service ยังคงอยู่


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/830646d138e49490d32bc3b65b055631.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=BDCOHzw%2FF3sk8J4h%2B9SolftuIxCEBXy2TsoAax8ewOUdzyUtOHZOtRHO1jrYfnSTKl%2FNclTVZX%2BR69SHeHrpk7q9qJWrsUP%2BPHm3ID6ZVCYdhkcTlD8JTibKOEdSyAM1HiqXctnIToYOWnzMN7HcwDi2Is%2B%2BQcffNHlv195yRUv0uO%2FNqaB8xVpHcEFLtf2gT5aXkCFqD2uRTLfQ4B5unMGD3%2FhCrnr2YfgHTUr4CxotNZZkT%2BnR1JPPKVhNahHzEbWGnQ%2BVexRtogG06c6XYiAfsaFe5101vbs8dgv0VjLhUZGSIInDGmhL23881tPyta1JnwxiR6CxN%2FX6oTaFzQ%3D%3D)


# Service - Load Balancer


ทำหน้าที่เป็น Internal Load Balancer 

- เมื่อ Traffic มาถึง Stable IP ของ Service, Component Kube-proxy จะทำ Load Balance ที่ Traffic นั้นไปยัง Pods ที่เป็นเป้าหมาย

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/5d9b080055e385cbbc373a2891033e65.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=clBiFmnzYkW8%2FAVzi7GPQ5hP3K71ILUOz5rDyymXZEGUxzx%2BQnyXIbkE2n%2BzTOBW3w%2BNXDQoFSu95EF0lzqXG0P%2BkBzJIlfpoYFSVTaCgHd5Yiso2LXl4KiZQwfEDQQcUzU8woZAhIlhmA1Tr4do63ehVeYh%2Fpesa7MPgY6L5W%2BoVx9Tes425QR4xrRgHgErNmzky%2FW35h9pyxqDzHfrPOmADYAVYqIGrEk1gvJBtaBExPSU7Hvn%2B7UuQ3W3f8N%2B0ieDOwhGwFOir%2B0jsrhd68sr467yMmB4o8JgOGj9uuZ2mo58C5Zp%2FYYtCKJ%2BZh2VR47alqpGm9e7PgikpLa15g%3D%3D)


# Service - Using for Inside and Outside of Cluster


การใช้งาน Stable IP และ Load Balancer จะขึ้นอยู่กับชนิดของ Service ที่เลือกใช้


| ประเภท Service      | Stable IP & Load Balancing                  | การใช้งานหลัก                                                   |
| ------------------- | ------------------------------------------- | --------------------------------------------------------------- |
| ClusterIP (Default) | มี Stable IP ภายใน Cluster                  | Inside Cluster Only (ใช้ติดต่อระหว่าง Services ภายในเท่านั้น)   |
| NodePort            | มี Stable IP และ เปิด Port บน Node          | Outside Cluster (เปิดช่องทางเข้าถึงจากภายนอกผ่าน Port ของ Node) |
| LoadBalancer        | มี Stable IP และ สร้าง Load Balancer ภายนอก | Outside Cluster (รับ Request จาก External Load Balancer)        |


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/1c7b663cb6abc6dd1dc51f9e50ea8b02.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=P2xGhNbxIfWpfnhcuy7u6h70En6RvWUdY0evRssDnY6ZlCRYRmx2gD4MJsmtDnW1uobprt3tDY91tkx5shqvkwnbP3NjwEVxxCSX8KIjb2BfM%2BHc2DUDJB02ATOCcR9Rfj9IRxcMOx7sP3tPJcwiEEnUV19N3gK1esKpSJrLUNIlt%2FvMMfcjoDHUZfT2MIz3EGqjHoPK6jNgbOhOR65Nn%2Bep%2FUFigWPD2%2BdXTpNPWHcjbHZRFSxedTP%2BjHNs66V%2FS4SgLopFBSMqrDva8VRgGZ7f2%2FszfAtOkwhUQdFkkmuZh8qsjuNWV74MnbA%2F%2BYSp9kS1%2B7xGBRaW%2FZGTrLh%2B6A%3D%3D)


# Internal Service


## ClusterIP Service


Expose service ให้ user อื่นๆ ที่อยู๋ใน cluster เดียวกันไปใช้ (เปิดเผย IP ภายใน Cluster)


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f9a547dba55706dac300831e305bcdf3.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=ThgLJL40sbs5WiYYEF7dzz0eFkjNoVn8oUdULpH6y3zZ0U7f45DxGvDPYs5tPNXYphJzL9xxp8RRQ%2BvdYa%2BC9Jd5%2BIDF0NhCccJMiRkKsj6LW4Y%2B%2FeaNbDsFpgDYOy4EOVPrOJSO2kKRnVtZPXZl5b6oBMvhry1U9xC%2FKyQKlShFpN%2BWNOHvY2gw3511Y2%2BXvyEudlIjTExCaZ%2Fr7D9pVR8jqnSSoNs8Er1kpQeHN2mdAc9oMpLB4OWc%2BakrRdpsOlIpYs%2BQHpVOLyw%2B1wYgobgGYi%2FxT4zO5COhb2eCxl87rDCUuniHd2C5fLRkVht90hxFXio9sjRreBSmJXRcuw%3D%3D)

- กำหนด mapping ip คือ protocol, port, targetPort,
- รับ Request จากภายนอก Client แล้วกระจาย Load, configure ผ่าน labels selector

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/29bc5c8e59441cbdcb1a9a2d74624eca.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=HLdPm%2FzBZ%2BMdqA2WRb4faDw5GXy5c%2FoL2G9hvfvcucsA%2Bf7KCCA32svHX01tft1hjOmUarEI7l2hm5uwbUHkHhO6ex%2F2sI0vKOXcKzJJ0OGfiGiqoQZQQLGx%2Fza1rcWA2mI6ipCxDsdrrkDn5PpsMUEj50z3qOPeiNKcnLb%2FqM9OkX2ealWwhshIzgdQh6f3Lz%2FXCZSlY9pbStHQBf02OBqKHp%2FJZnBHqkX1R8P3Xp%2ForJWOXdqRyzCVfC%2BcBHx99vswnkXR1h7T%2Bufz%2BU9t3CavJGAb%2BrUtBRceAEUvYEeQa34MaGQXvZRjOckg4FH%2BHNZRVCnoMMxrnzfQyEQk0g%3D%3D)

- สิ่งสำคัญคือต้องกำหนด labels selector, ports ให้ตรง และมีอยู่จริง/

## HEADLESS Service


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/8bb6b82cc4af6e9ffc97115447e3ad46.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=Goin4qdGo16fd5SrSUJWOky917RuCHHz3mhVQDC%2BgqL39%2F%2FxJ1EBOzA%2BdhWaUIPT%2BisXCYD8LN7EgcbW9Ir64LBAAEWG2Qp5ajUUSfcXShNaver7vKq07tW52OOSwrM7lw%2BiwzFnlBbFbE6iJtvfckKL8rCJVAOsbiMBvnssN1t%2Bfp8Bj0QiHd7jaHXdhIo3qLksazF4QyTaF35lRTwg3xXcbzvVK1heAlvxw0tCnW8iMquJDd97Zsosf4PNWwHBX0iPwoLWkDVuO7NcBevSCrxLpb81J2na82uqdxY2UD1Y6%2F0QwF13XYrwq5rzgXD%2FLDkJJh%2BtjxvLt%2FJkEAUiug%3D%3D)


Expose Service ที่ไม่ต้องการทำ Load Balancing, Single Service IP โดยสามารถใช้สิ่งที่เรียกว่า Headless โดยระบุ None ให้กับ IP ของ Cluster คือ spec.clusterIP: None

- ไม่ต้องการฟีเจอร์ Load Balance (ถ้าเกิดว่ามีคนเข้ามาใช้ แล้วเราอยากให้ใช้ node เดิมไปตลอด คนๆ นั้นพอมาใช้ใหม่ก็จะเข้าไปที่ Node เดิม, ส่วนใหญ่จะใช้กับ Stateful Application คือเราต้องการเก็บ logs เดิมกับ client เดิม ไม่ต้องการให้ client ย้าย request —> node ไปมาๆ

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/4566c7bcac3ec2bb6853f41ad51946e6.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=bl4PHqUP7QmI38%2BokQwK8agfO2U0ukFPsKAWLGDbkhDVQefvrDGfMbcwzqRXpoQv%2BIgNOamNeyo6uFzqC20Fh3YfHs%2F%2FzvXCC3FJIPmNiOtuMYK%2FXmmnvA0gxQNtbNkWRVBsQmchWKQvrVkyQ9Z3gxogYmhQLI9Iho9Rm4gsOCFzH52p6Zp587ZWAjVrqli2WblWPUmqihd92QzM2370bTt%2BozmnMT1OifFhF8PabelaxVHSjAsw6Lx%2BHkq%2B9KhzJgYbGVHiqHT998d0YqZuARG55v6i4G4WeYC3y76GDNKgXdkmzW2goLslpFcs6JOkI992V%2Bdf8Rx%2BRcitj6ol5A%3D%3D)

- ดูจาก diagram, client เข้าไปที่ pod และกำลังจะ commit, insert data แต่ว่าอยู่ดีๆ ไป connect อีก pod ทำให้ data มัน diff หรือ ไม่ตรงกัน แล้วไม่จำ state ด้วย จึงใช้ Headless มาช่วยโดยที่ client จะใช้ traffic เดิมที่ชี้ไปที่ pod นั้นๆ เสทอ เข้ามาใหม่อีกครั้งก็ไปที่ pod เดิม

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/7fb0d42017618222f55676ea4111b5f1.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=bQmUqetsJtBOVhMrWeGh9wkGto2C2MdMwfUarcyK%2BAduhmJd9vrtjfPRnsaEMzJVB0JqRBqlQ6ugZMsSZ0dFYh%2BNcKPNMiZ59DKqQo6WbVWhzOuVpYXAl0ABBEotWWaNoV%2B%2F%2Fj0XxKCLiJsZiKf04OLnUAhRH9E%2Fo2ZW7j0R%2BncWET%2Fsc5fASrW%2F2bsGxSTdJLHb%2FS8RZYk2VKYd0QUBbHGIxJs9P1%2Bf%2FrUsOKjCgnYdH23ejLLEC%2BT31nnkH6N4OTHDpQMApgDcVkgrHXfcXSieS0gwqhaeUbUl5lUtQ2ifmmWtCF86pcP%2FdtdlxhTaov6D4KPelWbFu7z0P0k3og%3D%3D)


# LAB


## Single Deploy


01-simple.yaml


```yaml
apiVersion: apps/v1
kind: Deployment # Deployment 
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
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-web-ingress
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-web-service
            port:
              number: 80
```

- เราสามารถที่จะแยกแต่ละ Object ได้โดยใช้ - - - ใน yaml syntax จะถือว่าเป็นคนละไฟล์
- จากนั้นใช้คำสั่ง `kubectl apply -f 01-simple.yaml`
- และเข้าหน้าเว็บ [`localhost:8888`](http://localhost:8888/) จะได้หน้า nginx ขึ้นมา

## Multiple Deployment


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
  labels:
    app: my-nginx-deployment
spec:
  replicas: 1
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
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-https-deployment
  labels:
    app: my-https-deployment
spec:
  replicas: 1
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
        name: my-web-on-httpd
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
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-web-ingress
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-web-service
            port:
              number: 80
```

- จะเห็นว่ามีการกำหนด Deployments ไว้ 2 Pod
- เราจะใช้ Service ชี้ไปที่ Label my-web ซึ่งเป็น label ของทั้ง 2 deployment และจะเป็นการทำ Load Balance
- จากนั้นเข้าไปที่ [`localhost:8888`](http://localhost:8888/) จะได้หน้าเว็บที่แตกต่างกันทุกครั้งที่รีเฟรช

## HEADLESS


เราไม่อยากให้ Service เราทำ Load Balance อยากให้ User เข้าเครื่องเดิมเสมอ


ถ้าเป็น HEADLESS, K8s จะสร้าง Routing Table ของ Pod เลยจาก Service ไปที่ Pod โดยตรง


ส่วนใหญ่ HEADELESS จะใช้กับ Stateful Services 


```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-web-ingress
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-web-service
            port:
              number: 80
---
apiVersion: v1
kind: Service
metadata:
  name: my-web-service # non-headless service, normal service
spec:
  ports:
  - port: 80
    name: web
  selector:
    app: nginx
---
apiVersion: v1
kind: Service
metadata:
  name: my-web-service-headless
spec:
  clusterIP: None # defines a headless service
  ports:
  - port: 80
    name: web
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx # has to match .spec.template.metadata.labels
  serviceName: "nginx"
  replicas: 3 # by default is 1
  minReadySeconds: 10 # by default is 0
  template:
    metadata:
      labels:
        app: nginx # has to match .spec.selector.matchLabels
    spec:
      terminationGracePeriodSeconds: 10
      initContainers:
      - name: init-nginx
        image: nginx
        command:
        - bash
        - "-c"
        - |
          set -ex
          # Generate mysql server-id from pod ordinal index.
          [[ `hostname` =~ -([0-9]+)$ ]] || exit 1
          ordinal=${BASH_REMATCH[1]}
          echo $ordinal > /usr/share/nginx/html/index.html
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
  labels:
    app: client-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-client
  template:
    metadata:
      labels:
        app: my-client
    spec:
      containers:
      - image: nginx
        name: my-client-on-nginx
        ports:
        - containerPort: 80
```

- logs

```yaml
$ kubectl get pod --watch
NAME                                 READY   STATUS    RESTARTS   AGE
client-deployment-68bd59fc95-mr5mf   1/1     Running   0          29s
my-web-deployment-799cd6f867-gpzhg   1/1     Running   0          37m
my-web-deployment-799cd6f867-n5sm4   1/1     Running   0          37m
web-0                                1/1     Running   0          29s
web-1                                0/1     Pending   0          0s
web-1                                0/1     Pending   0          10s
web-1                                0/1     Init:0/1   0          10s
web-1                                0/1     PodInitializing   0          14s
web-1                                1/1     Running           0          17s
```

- จะเห็นว่าชื่อของ pod ที่มาจาก deployment ที่เป็น Stateful จะได้ชื่อตรงตาม Service เลย
- ทดลองเข้าถึง Service

```yaml
$ kubectl exec -it client-deployment-68bd59fc95-mr5mf -- curl my-web-service
0
$ kubectl exec -it client-deployment-68bd59fc95-mr5mf -- curl my-web-service
2
$ kubectl exec -it client-deployment-68bd59fc95-mr5mf -- curl my-web-service
2

$ kubectl exec -it client-deployment-68bd59fc95-mr5mf -- curl my-web-service-headless
1
$ kubectl exec -it client-deployment-68bd59fc95-mr5mf -- curl my-web-service-headless
1
$ kubectl exec -it client-deployment-68bd59fc95-mr5mf -- curl my-web-service-headless
1
```

- ถ้าเราเข้าถึงตัวของ Service ที่ไม่มี HEADLESS จะเป็น load balance ไม่จำ state
- แต่ในส่วนของ HEADLESS จะได้ state เดิมเสมอ แต่จะไม่มี CLUSTER-IP ให้

```yaml
$ kubectl get service -o wide
NAME                      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE   SELECTOR
kubernetes                ClusterIP   10.43.0.1      <none>        443/TCP   14h   <none>
my-web-service            ClusterIP   10.43.239.16   <none>        80/TCP    11m   app=nginx
my-web-service-headless   ClusterIP   None           <none>        80/TCP    11m   app=nginx
```


### Describe Headless


```yaml
$ kubectl describe service my-web-service-headless
Name:                     my-web-service-headless
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=nginx
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       None
IPs:                      None
Port:                     web  80/TCP
TargetPort:               80/TCP
Endpoints:                10.42.3.14:80,10.42.2.10:80,10.42.0.8:80
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>
```

- จะไม่มี IP Address เป็นของตัวเอง ถึงแม้ว่า Type จะเป็น ClusterIP อยู่

```yaml
$ kubectl describe service my-web-service
Name:                     my-web-service
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=nginx
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.43.239.16
IPs:                      10.43.239.16
Port:                     web  80/TCP
TargetPort:               80/TCP
Endpoints:                10.42.3.14:80,10.42.2.10:80,10.42.0.8:80
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>
```

- เมื่อลองเทียบกับ Service ที่ไม่ได้กำหนด HEADLESS จะมี IP เป็นของตัวเอง
