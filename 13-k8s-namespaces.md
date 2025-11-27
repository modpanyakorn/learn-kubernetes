
# What is NAMESPACES


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/716d2ca328e773d1b36a741d869efd88.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=NZTayTl2p66ssnZsFHqiSAJxFgfeAe6he5pmYJO6ZpV23PUoC0%2FhoV5dilZQsRSO71FHSKg%2BI0mDVPrPzjUR64Urq4ooDAafD6KyroPulGJiJZOt54GhqSi755m1SfLdMwJ7tRg0nRFiGxNKSoBFn3B95jnHJvQJ9yErszaOSgG%2BfOl7fMvjU7u%2BcA%2FZsArH%2BA6228EPiLh3cuYTMM6G5504ZeYukRbDqxANA8HCEe4UHk4B7NCcrhLfXqF5JDzMnjT0C4LeGg2oxnxA8ysDB1MdHvBNCv2d9NoXv7SHw8vaOaxFrYmzZrwrMR3lJqyKAz%2FjqnZR5z9iQTfGNztVHQ%3D%3D)


Namespaces ทำให้สามารถแบ่ง Cluster ย่อย หลายๆ cluster ได้ (เป็นเครื่องมือในการจัดแบ่งส่วนของ Resource ใน Cluster)

- Namespace แขก Cluster แบบ Logical ภายใน Cluster เดียวกัน
- สามารถที่จะใช้ kubectl แยกตาม Environment ได้โดยระบุ Namespace, อีกทั้งเรายังตั้งชื่อ Resource ซ้ำกันได้ได้ ตราบใดที่อยู่คนละ Namespace
- Namespace: default ทุกคำสั่งที่เราทำงานไป object มันจะไปอยู่ใน Namespace: default

## Namespace Types


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a6dffc7183db0cbbf2c80dcd3a725c6d.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=VsVhbyBP0m5b%2FX%2Bi%2BsxQ3XxFk83eo3a6Ewt%2BUBSzQjpkF62rHKy4EaNfBxxI9pfz5JE2%2FF%2BVQ3ZsfkvEDa9BTjngP5UaoD17j2z%2FTN%2FxnjaFRj26%2BJyhd%2FFAFyl%2F6E8NawlfvldEcWce34E5mxN56uQJs5iYtFw17CPKJCV%2B8fcFvUQ2tfrvqO1kOFEWF7rB9mALGtdVpQ6jPtKsN6q95arbhVxXL8RZ%2BsFJk9MGnwhhNVrtivb1VXqikvArnV37H9D0JbfH8I2ZnyUUl82psDwiCP%2BWkXphlHlEidwfMri4x5PYNEsVnTEPpSC8wo5L3n%2FbnxOW74DwvWyzbfqHIg%3D%3D)

- kube-system: Namespace ที่ให้ Components หลักใช้ เช่น kube-scheduler, coreDNS เราไม่ควรไปยุ่ง
- kube-public เป็น Namespace ที่มี Resource เข้าถึงได้แบบ public เช่น ConfigMap อยู่ใน kube-public (User ใน cluster สามารถอ่านได้ มักจะใช้เก็บ ConfigMap)
- kube-node-lease: คือ Health Check ของ Node ดูว่า Node ไหนพร้อม/ไม่พร้อม ทำงาน ดูว่า object ทำงานถูกต้องอยู่รึป่าว (ดู Actual Current, Configure เทียบกัน)
- default: object และ resource ของเรา หากไม่ได้ระบุ namespace เองใน configure

# Use Case - Single Namespaces


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/5804140c0db97f497b1ab4084ac4dc23.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=HuMEXF%2FqvVmMi1LJBH99BXJG6CAAZDSOmOeaKMl4oyIxh%2FCDqA4bxMaELNngCnoOjjRuKWTo2AFqhwM%2FG%2BPCL0TxNR6%2FnxyM6FCfAmD%2BITtHhdLFM0b6BjbhucJRwytJYLn0GR7UgGhPDPG%2BhRVT8t18Q6xI%2FH%2BlNZYLejmf7Dhq1zsr24TM88LdFb%2F6x7ka0XIfxso9SdVqjn1h3wQMOc67SMb9lDnyF6ZaIgOGUkavbw3x3V8yLmq9hEQT8cNa%2BJTnEkc3L9Xi0VfZLKudk%2FmsbB5IW6UZn7fYURUSqxkzio87DBN0mJ9rD8bY0eEorvCbQ20OyrsHlTcFNkES3Q%3D%3D)

- หากมี object, componenet ไม่ซับซ้อน หรือ น้อย สามารถใช้ Single Namespace ที่ไม่ได้จัดกลุ่ม

# Use Cases - Resources/Services Grouped Namespaces


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/7492ac49feaf14e5313527ed92bb18c7.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=ohfBcZZGw2TsNv8QNgfQWf7hpPcPoiuKglgk778yUB%2BiHl%2BYYkNUW84ZEu5s2c8h7sgjUjpWdX4YylKe1AWfOz41N24kNWTa44Jc1fdjFMxGGcJsXXDK7%2BeFXp6S%2FISKAcpDx9RW0dwvSCAqga5PRTmEgrxEk%2F50omg1E7pfWHaFErhnZuqbgXoe0LHbU%2BwdYbHe3%2F4OyXPGGEMgP1N5G061oDydV%2Ba5H9L56sh3aOfWkz8DXCQpvIiNTLnYulRucGlsSYBz4huTtoxBfkPcE9Lcs%2BmR8yK%2B49Hh4wUFlXVZh%2F4yO1RWG50ym%2BGpFOnjm3neDLxhbFtuy%2F3047gTTA%3D%3D)

- แยก Namespace ตาม Resource/Service
- ช่วยทำให้ Cluster จัดการได้ง่ายขึ้น และเป็นสัดส่วน

# Use Case - Same App/Diff Team Namespaces


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b105e5d7cf0eb92424982c09a7c0382f.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=JpaNLJimTDdAg6ibedsPdKqQx0%2Bc1oeuwbMsd5v04TJ5seLmWTSYPhc%2FSdfNq4Dd%2BLKJtOKGmSGCrwdY8QOm2mmAUqYAPvC8I5NLQrEfnlR9K%2FuaWMZ%2BUNGYa8PYWiOeLXtcsBxR12IFDpC8QyDC1auDS1luQbwH3qnNQyuPdeowzIap7jiEbyd5XKaLbz6Uqlf3cDyk%2F9DwXMyPGrBwdQEg9EnsSoVGJhSXMSpudy8KCxIxpD%2FvyrHEnuqot49QDeLY0q2z5pyCbeW%2BnGAXXU8hjqMuzrhlJfBGqhKN%2BVYfQQ%2BuFkJC6SNPQA41Ci56pqAlFHWkuUD5l%2BszhwUN6A%3D%3D)

- ถ้ามีทีม Development มากกว่า 1 ทีมที่ต้องการ Dev Application เดียวกัน
- โดยแบ่ง Namespace ตามทีม เพื่อทำงานอิสระต่อกัน
- Deploy/Dev เฉพาะ Environment ของตัวเอง

# Use Case - Diff Environment and Share Resources


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b2217e0e8f2fe204f679390b6e2fa739.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=KtKKoegOJT5d0R1%2FFeRVulLkK9PJSiCjI5DJ1CzItPZToTKPslxHVVxGLwHaP7xXRYyMJbIKjMbv6SeKhH%2F68%2B8S2sCUPebB48sRkGut%2B0zGczyG5k%2FnXcM7MVqwNkiW5fDChQF8lKzq%2BPztjujhYQ27SVDDhzSys3Uv%2FQuHD09k%2FKezTsfZg8g2Hxz7V54GVSd8r%2BQNrYBwMocN3ZIKyOfeyyG6bwDq8j3QpZ1p4g%2FYBz1tIGIelhQdM3R0WN3TFz6cF%2BL8yVLYSOckNnpCHqvf8PtutMShXOqsHeLlLBcynErf2zabaEAaM3QtIX%2F73eCiHbKmIk8LpwOzGuK4RA%3D%3D)

- ต้องการแยก Environment ในการพัฒนา Application ตัวเดียวกัน
- โดยใน Application เดียวกันสามารถแยกได้หลาย Copy หลาย Environment
- โดยจะได้ Application เหมือนกันตาม Specs ที่กำหนดไว้

# Use Case - Blue/Green Deployment


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/4cece2a2a972d6c7753418a0ab148012.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=CgGb5e2KZy3uYgiep3oFWvGKRChM4H2yXmQdyRZxXADL5wjMUFJlgyjlMzYiaH3CUU0OkNx%2FdzVx8m1sVsixJu26%2FLDraTlxgl%2Bdzvx8uFe03D6c6pmKP%2BWMVm59OJGz56UFYC8Q8Az%2FKOI4yfrYdYcfjpDA8EJyiu%2B1nBePiLbcDLKf68PCF%2Fi%2B9Y1n52TeB5o8zttnnE0FLXYfVVxTsvnkTgwFl%2FxGFfmASEEi1V9L1jjP02D7xxJ2nl1SfgNy8Kag1dIUMWWTrxgr4ha2tOLoWl19hg7IXTbhtToA8btuU8e%2FJRhji7F7lsceeyVZDxCbK9SaeoSFqdvjYgsgJg%3D%3D)


# Use Case - Access/Resource Limits


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/2b009b2c3f493100a1fa0742f2bfa57a.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=cNfOuA6e50F0UFduxWs1%2BhnKPN0zlEVPT5mLcyomV2QlG9gP1m4%2BdW6v%2Bgjk9D%2FboHAk9xt%2BJrcrdXHvhEz0w%2FMc3p6zywRLhfGy04mtaUSKi6MSm2v%2FX4t8gi%2B7of3nZ3Jv8p9WaepV6jx6gSw9qbg033415PMMO8RDQofNf5ePHFrbtF81r6DlOIHPMV6FfXW95vGo7WNxq4j%2BRZW48xDq7sg%2Frq5CUNrEoHcID7EL0eBwtC4HsQvr0B%2BAo6xOP9L%2BNP2KUc4FTh4RxInkOK5EDeRjJYgG5AWngBhRGdi045%2F3c4b%2FsPWg9De1%2Bp3Ea%2Fg4kQIYA7qJg2mg0YxLWg%3D%3D)

- เราสามารถกำหนด Resource Limit, Access Limit ไว้
- เช่น มี Dev ในทีมกำลังทำงานพร้อมกันเยอะสามารถที่จะ Limits Access กัน User ภายนอก หรือ Limit Resource เพื่อ share ให้ Namespace อื่นใช้ได้

# Common User Case for using Namespaces


การนำ Namespace ไปใช้ส่วนใหญ่จะพิจารณาจากสิ่งต่อไปนี้

- Structure: โครงสร้างของ Application
- Avoid Conflict: ลด Conflict ให้ทีม
- Share Service: Share Service ต่าง Environments
- Access/Resource Limit: กำหนด Limit Access/Limit Resource

---


# LAB


## Show Namespace


```bash
$ kubectl get namespace
NAME              STATUS   AGE
default           Active   3h19m
kube-node-lease   Active   3h19m
kube-public       Active   3h19m
kube-system       Active   3h19m
```


## Create Namespace


สร้าง Namespace ใหม่ด้วย `kubectl create namespace <namespace>`


```bash
$ kubectl create namespace manual-namespace
namespace/manual-namespace created

$ kubectl get namespace     
NAME               STATUS   AGE
default            Active   3h20m
kube-node-lease    Active   3h20m
kube-public        Active   3h20m
kube-system        Active   3h20m
manual-namespace   Active   2s
```


หรือใช้ Script ในการสร้าง


```yaml
# my-namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
  labels:
    name: my-namespace
```

- จากนั้นสั่ง `kubectl apply -f my-namespace.yaml`

```bash
$ kubectl get namespace
NAME               STATUS   AGE
default            Active   3h23m
kube-node-lease    Active   3h23m
kube-public        Active   3h23m
kube-system        Active   3h23m
manual-namespace   Active   2m57s
my-namespace       Active   48s
```


ถ้าเราสั่งดู Pod โดย default จะไปดูตัวของ default namespace


```bash
$ kubectl get pod
No resources found in default namespace.
```


กำหนด flag `--namespace=<namespace>`  หรือ `n` ในการดู Namespace ที่ต้องการ


```bash
$ kubectl get pod --namespace=my-namespace
No resources found in my-namespace namespace.

$ kubectl get pod -n my-namespace 
No resources found in my-namespace namespace.
```


## Show all namespace with resource

- Pod

```bash
$ kubectl get pod --all-namespaces
NAMESPACE     NAME                                      READY   STATUS      RESTARTS   AGE
kube-system   coredns-ccb96694c-wngrz                   1/1     Running     0          3h29m
kube-system   helm-install-traefik-crd-ctfmw            0/1     Completed   0          3h29m
kube-system   helm-install-traefik-p6skg                0/1     Completed   1          3h29m
kube-system   local-path-provisioner-5cf85fd84d-dbplc   1/1     Running     0          3h29m
kube-system   metrics-server-5985cbc9d7-pqtc6           1/1     Running     0          3h29m
kube-system   svclb-traefik-69f9a380-jh8nk              2/2     Running     0          3h29m
kube-system   svclb-traefik-69f9a380-kp6b5              2/2     Running     0          3h29m
kube-system   svclb-traefik-69f9a380-r8zsm              2/2     Running     0          3h29m
kube-system   svclb-traefik-69f9a380-rqb5j              2/2     Running     0          3h29m
kube-system   traefik-5d45fc8cc9-l2ddj                  1/1     Running     0          3h29m
```

- All Resources

```bash
# ไม่ดชกำหนด Namespace
$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   3h30m

# กำหนด All Namespace
$ kubectl get all --all-namespaces
NAMESPACE     NAME                                          READY   STATUS      RESTARTS   AGE
kube-system   pod/coredns-ccb96694c-wngrz                   1/1     Running     0          3h30m
kube-system   pod/helm-install-traefik-crd-ctfmw            0/1     Completed   0          3h30m
kube-system   pod/helm-install-traefik-p6skg                0/1     Completed   1          3h30m
kube-system   pod/local-path-provisioner-5cf85fd84d-dbplc   1/1     Running     0          3h30m
kube-system   pod/metrics-server-5985cbc9d7-pqtc6           1/1     Running     0          3h30m
kube-system   pod/svclb-traefik-69f9a380-jh8nk              2/2     Running     0          3h30m
kube-system   pod/svclb-traefik-69f9a380-kp6b5              2/2     Running     0          3h30m
kube-system   pod/svclb-traefik-69f9a380-r8zsm              2/2     Running     0          3h30m
kube-system   pod/svclb-traefik-69f9a380-rqb5j              2/2     Running     0          3h30m
kube-system   pod/traefik-5d45fc8cc9-l2ddj                  1/1     Running     0          3h30m

NAMESPACE     NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP                                   PORT(S)                      AGE
default       service/kubernetes       ClusterIP      10.43.0.1      <none>                                        443/TCP                      3h30m
kube-system   service/kube-dns         ClusterIP      10.43.0.10     <none>                                        53/UDP,53/TCP,9153/TCP       3h30m
kube-system   service/metrics-server   ClusterIP      10.43.191.93   <none>                                        443/TCP                      3h30m
kube-system   service/traefik          LoadBalancer   10.43.23.184   172.18.0.3,172.18.0.4,172.18.0.5,172.18.0.6   80:31732/TCP,443:32403/TCP   3h30m

NAMESPACE     NAME                                    DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
kube-system   daemonset.apps/svclb-traefik-69f9a380   4         4         4       4            4           <none>         
 3h30m

NAMESPACE     NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/coredns                  1/1     1            1           3h30m
kube-system   deployment.apps/local-path-provisioner   1/1     1            1           3h30m
kube-system   deployment.apps/metrics-server           1/1     1            1           3h30m
kube-system   deployment.apps/traefik                  1/1     1            1           3h30m

NAMESPACE     NAME                                                DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/coredns-ccb96694c                   1         1         1       3h30m
kube-system   replicaset.apps/local-path-provisioner-5cf85fd84d   1         1         1       3h30m
kube-system   replicaset.apps/metrics-server-5985cbc9d7           1         1         1       3h30m
kube-system   replicaset.apps/traefik-5d45fc8cc9                  1         1         1       3h30m

NAMESPACE     NAME                                 STATUS     COMPLETIONS   DURATION   AGE
kube-system   job.batch/helm-install-traefik       Complete   1/1           37s        3h30m
kube-system   job.batch/helm-install-traefik-crd   Complete   1/1           33s        3h30m
```


หรือใช้ `-A`


```bash
# All Resource
$ kubectl get all -A
NAMESPACE     NAME                                          READY   STATUS      RESTARTS   AGE
kube-system   pod/coredns-ccb96694c-wngrz                   1/1     Running     0          3h32m
kube-system   pod/helm-install-traefik-crd-ctfmw            0/1     Completed   0          3h32m
kube-system   pod/helm-install-traefik-p6skg                0/1     Completed   1          3h32m
kube-system   pod/local-path-provisioner-5cf85fd84d-dbplc   1/1     Running     0          3h32m
kube-system   pod/metrics-server-5985cbc9d7-pqtc6           1/1     Running     0          3h32m
kube-system   pod/svclb-traefik-69f9a380-jh8nk              2/2     Running     0          3h31m
kube-system   pod/svclb-traefik-69f9a380-kp6b5              2/2     Running     0          3h31m
kube-system   pod/svclb-traefik-69f9a380-r8zsm              2/2     Running     0          3h31m
kube-system   pod/svclb-traefik-69f9a380-rqb5j              2/2     Running     0          3h31m
kube-system   pod/traefik-5d45fc8cc9-l2ddj                  1/1     Running     0          3h31m

NAMESPACE     NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP                                   PORT(S)                      AGE
default       service/kubernetes       ClusterIP      10.43.0.1      <none>                                        443/TCP                      3h32m
kube-system   service/kube-dns         ClusterIP      10.43.0.10     <none>                                        53/UDP,53/TCP,9153/TCP       3h32m
kube-system   service/metrics-server   ClusterIP      10.43.191.93   <none>                                        443/TCP                      3h32m
kube-system   service/traefik          LoadBalancer   10.43.23.184   172.18.0.3,172.18.0.4,172.18.0.5,172.18.0.6   80:31732/TCP,443:32403/TCP   3h31m

NAMESPACE     NAME                                    DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
kube-system   daemonset.apps/svclb-traefik-69f9a380   4         4         4       4            4           <none>         
 3h31m

NAMESPACE     NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/coredns                  1/1     1            1           3h32m
kube-system   deployment.apps/local-path-provisioner   1/1     1            1           3h32m
kube-system   deployment.apps/metrics-server           1/1     1            1           3h32m
kube-system   deployment.apps/traefik                  1/1     1            1           3h31m

NAMESPACE     NAME                                                DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/coredns-ccb96694c                   1         1         1       3h32m
kube-system   replicaset.apps/local-path-provisioner-5cf85fd84d   1         1         1       3h32m
kube-system   replicaset.apps/metrics-server-5985cbc9d7           1         1         1       3h32m
kube-system   replicaset.apps/traefik-5d45fc8cc9                  1         1         1       3h31m

NAMESPACE     NAME                                 STATUS     COMPLETIONS   DURATION   AGE
kube-system   job.batch/helm-install-traefik       Complete   1/1           37s        3h32m
kube-system   job.batch/helm-install-traefik-crd   Complete   1/1           33s        3h32m

# Pod
$ kubectl get pod -A
NAMESPACE     NAME                                      READY   STATUS      RESTARTS   AGE
kube-system   coredns-ccb96694c-wngrz                   1/1     Running     0          3h32m
kube-system   helm-install-traefik-crd-ctfmw            0/1     Completed   0          3h32m
kube-system   helm-install-traefik-p6skg                0/1     Completed   1          3h32m
kube-system   local-path-provisioner-5cf85fd84d-dbplc   1/1     Running     0          3h32m
kube-system   metrics-server-5985cbc9d7-pqtc6           1/1     Running     0          3h32m
kube-system   svclb-traefik-69f9a380-jh8nk              2/2     Running     0          3h32m
kube-system   svclb-traefik-69f9a380-kp6b5              2/2     Running     0          3h32m
kube-system   svclb-traefik-69f9a380-r8zsm              2/2     Running     0          3h32m
kube-system   svclb-traefik-69f9a380-rqb5j              2/2     Running     0          3h32m
kube-system   traefik-5d45fc8cc9-l2ddj                  1/1     Running     0          3h32
```


## Set Current Namespace


ใช้คำสั่ง `kubectl config set-context --current --namespace=<namespace>`


```bash
$ kubectl config set-context --current --namespace=manual-namespace
Context "k3d-my-cluster" modified.

# จะเห็นว่าพอเรา get pod มันจะเปลี่ยน namespace defualt -> manual-namespace ที่เรา setting ไว้
$ kubectl get pod
No resources found in manual-namespace namespace.

# สามารถที่จะเปลี่ยนกลับไปเป็น default ได้
$ kubectl config set-context --current --namespace=default
Context "k3d-my-cluster" modified.
```


## Delete Namespace


```bash
# CLI
$ kubectl delete namespace manual-namespace
namespace "manual-namespace" deleted

# ลบด้วย Script ที่ใช้สร้าง Namespace นั้นๆ
$ kubectl delete -f my-namespace.yaml 
namespace "my-namespace" deleted
```


### ไม่สามารถลบ Default Namespace ได้


```bash
$ kubectl delete namespace default
Error from server (Forbidden): namespaces "default" is forbidden: this namespace may not be deleted
```


เพราะมันถูกกำหนดโดย Kubernetes ให้เป็น Namespace ที่สำคัญและเป็นค่าเริ่มต้น ของระบบ ถ้าเรารัน kubectl สร้าง resource แบบไม่กำหนด Namespace มันจะใช้ Default แล้วนำ object ที่สร้าง เป็นของ Default namespace



