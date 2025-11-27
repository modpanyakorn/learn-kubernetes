
# Publish Service (Service Types)


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/73da74b41ed1c412dd5bbaf5590bab1d.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=SGRLVL3gFyYgqxiE6AWeBS7I6nJXFNbWT6ocubh30lsIS7d6%2BgTGxRtjov34HNGFSmiwRWxvlSVc%2FGX3Oi4wmP4UQecBDaVbFeHXYK9TElfXE2Vh5mqPylMlHd%2FqHtCkFEVAosk976117%2BlZRbH9hfn4PUE5ZKdhtNxI4UzMlFuqfyiiA7Obbz8MIPfzJv82QcTAflU5SYAadkTvHx8DE7TykNoe%2B7NpvrQE97OfOqF7rx8On9KJbEWfrUyTfkLjEmM%2FzmdNiKcUQkFnf2S53QgezKmcME7uPutLZqE1qqRzjhDwB9677glPz1ghAJInDlRL%2BcTbeLZbyDgo2nrmVQ%3D%3D)

- ClusterIP
	- สำหรับ Internal Service
- NodePort
	- สำหรับ External Service
	- สามารถเปิดช่องทางจากภายนอกให้เข้ามาได้
- LoadBalancer
	- สำหรับ External Service

# NODEPORT SERVICES 


NodePort จะ Expose ตัว Service ของเราผ่านทาง Node IP สามารถทำให้ user ภายนอกเช่น web browser, external tools, cluster network สามารถเข้าถึง Service ของเราได้


![8bf9d10f-2318-4931-b09b-ac19c9805314.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b66cc6bb2914f1cb6a6a2d49340f94a0.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=FRfMPgqYZ1u9zKYmq2%2F5ViOM24DollX5X%2F4szYKlM98jThuvEHkB93U%2BTFyRRctTAoSdzTBAgUKPFpz3lA2u3MbUmiYONrz%2BRmZoePZdadHJoSlCWMYrsywKWPA%2B1oGrA%2FXb4yB6rsQtu1EyYuazSaNQjoRNeXoX1vur3Q75TEnn5w2UWWnXdQ%2BSB848JfUm8jEtnosRbiWGaSoTA5LS49xEekM8NmMbORXfeQA2sPrLvXys2e3SETBZauPCF5xacyQn6C%2B0de2hbCxmZ9%2FSpc6SUHf%2FNCnu6AWp871tcTTBcYu9X3th6%2Bd90pE4HR85dgzxPXDyNr2%2BycinWDmlXA%3D%3D)

1. Client ส่ง traffic ไปที่ IP ของ Node ที่ port 30080
2. Kubeproxy ที่รันบน Node จะ ฃตรวจับ traffic ที่ port 30080 และ forward ไปยัง cluster ip และ port ของ service 8080
3. Service ทำหน้าที่ Load Balance โดยการเลือก Pod และส่งต่อ traffic ไปยัง Pod โดยใช้เส้นทางของ port 80

$$
\text{Client} \xrightarrow{IP\_Node:30080} \text{Node} \xrightarrow[\text{kube-proxy}]{Forward} \text{Service:8080} \xrightarrow{\text{Load Balance}} \text{Pod:80}
$$


แต่ว่าเราต้องพยายามหลีกเลี่ยงอย่าใช้ NodePort เพราะเรากำลังเปิด Port บนแต่ละ Server (Physical Machine, Logical Machine)


# LOADBALANCER SERVICE


เปิดให้ Application เข้าถึงได้จากภายนอก Cluster และสร้าง External Load Balancer ทำงานร่วมกับ Cloud Provider ที่ใช้รัน Cluster โดย Client จะเข้าถึง public ip ของ External Load Balancer


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/46353aa3ff20ae3d4bb220588c60a875.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=E8bLikNxEnZyEcr3vSLELgBJ5jiRBL1pbGQuq9y4gudSsamymx%2F74dRyC%2F1KPrsNq9LUMvVLHOoGK%2B0qTdEDTjQcRg%2FTsXls%2F8srnipvmJx9HFnr724uPwnehRJphK%2FQvGU%2FhWbHGXiUIJrFPOkCeONdtihSGGwHdhWwbXBiwdeG0IefjZcY1%2FYXTZ2ncSQQTp6e4zf86mmxdC0%2FxroimBR1oUWjQqrBLHNVfS5wG%2FdpN5uxHPS3r1PR4y23TTwz%2Bh2vc4N2eQR2nqjdxLvab7OV7d9e8SpwUqe9bHDC57N2MZkcebI28BP%2FWubJrtjoq0dgnW1JHnMeeEQmIxNKgg%3D%3D)


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/d5287829bde57ba6a1109a6d648963ef.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=iYnRnoNT1kSgQy2cziA%2Fl05RMqNGYhKEhGkxCU3A9praINjMbe4GV34fiCnUsPdpNBD4UbQgHlPwAQ%2FcaPDnWdJz%2FQ1A718zeyZ1ISko%2Fks8Ytsc8%2B919cvVJA0eOs5d7%2BT1yZfQ8yVLN0dpCkUWugz3vrPT6emrzRP1lX0kS6YhelfpFOv44q8hiiLYKBEl0Np5RxPds8AVoKelVp%2F3Xtiw8eig8t1Ddj%2FlKuP5ff9Uu0QLHso%2FQoBLocyhtdLXZ6X%2BSpFOJmhUW2g53XPtuvuACdEAwuqPl5rQ83veMW4%2Bfd%2FOjYbirjIjP%2Bvnu7gujUVie%2B2jBbn6VEpPCX893A%3D%3D)

- LoadBalancer จะยอมให้ Request ที่วิ่งเข้ามาจะยอมให้แค่ External Load Balancer ที่เรา Setup ไว้เท่านั้น
- จะการันตี ได้ว่าเราไม่ได้เปิด Port อื่นๆ ให้ใช้ connect ได้ ซึ่งจะปลอดภัยระดับนึง

# EXTERNL NAME/IP


Service เราอยากติดต่อด้านนอก แต่ไม่ต้องการใช้ IP, Port อยากใช้แค่ Name ในการระบุที่อยู่ เช่น


Example: 

- Pod ที่รัน Application ต้องการติดต่อกับ DB ที่อยู่ด้านนอก เราสร้าง Service หนึ่งตัวที่คอยติดต่อกับ External DB ตัวนั้น

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ex-json-service
  namespace: prod
spec:
  type: ExternalName 
  externalName: jsonplaceholder.typicode.com # ชี้ไปยัง Domain ภายนอก
```

- ถ้ามี Request เข้ามาที่ ex-json-service ให้ forward ไปที่ “jsonplaceholder.typicode.com” ได้เลย

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  ports:
    - name: http
      protocol: TCP
      port: 80        
      targetPort: 9376 
  externalIPs:
    - 80.11.12.10 # External IP ที่ Service จะรับ Traffic เข้ามา
```

- ถ้าเราไม่รู้ Name เรารู้แต่ IP ก็สามารถใช้ key “externalIPs” ได้

# LAB


## NodePort

1. สร้าง Cluster ใหม่

	```bash
	$ k3d cluster create my-cluster-nodeport \
		-p "8888:80@loadbalancer:0" \
		-p "30080:30080@agent:0" \
		-p "31080:30080@agent:1" \
		--agents 2
	```

2. `kubectl apply -f 01-simple-nodeport.yaml`

```bash
$ kubectl get service -o wide
NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE     SELECTOR
kubernetes       ClusterIP   10.43.0.1      <none>        443/TCP        11m     <none>
my-web-service   NodePort    10.43.143.42   <none>        80:30080/TCP   5m36s   app=my-web
```


```yaml
Name:                     my-web-service
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=my-web
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.43.143.42
IPs:                      10.43.143.42
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30080/TCP
Endpoints:                10.42.0.5:80,10.42.2.5:80
Session Affinity:         None
External Traffic Policy:  Cluster
Internal Traffic Policy:  Cluster
Events:                   <none
```

- จะเห็นว่ามี Service Type: NodePort, มีการระบุว่า PORT ที่ใช้มีการ Mapping ไปที่ Node อะไรบ้าง
- จะเห็นว่า Endpoint คือ Virtual IP ของ Pod มันจะไป Mapping กับ Endpoint (Pod Address) ที่ได้ configure ไว้ เช็คจาก

	```bash
	$ kubectl get pod -o wide
	NAME                                 READY   STATUS    RESTARTS   AGE   IP          NODE                               NOMINATED NODE   READINESS GATES
	my-web-deployment-799cd6f867-d56c4   1/1     Running   0          10m   10.42.0.5   k3d-my-cluster-nodeport-server-0   <none>           <none>
	my-web-deployment-799cd6f867-nmdf4   1/1     Running   0          10m   10.42.2.5   k3d-my-cluster-nodeport-agent-1    <none>           <none>
	```

- ลองเข้าไปที่ [localhost](http://localhost/) port: 8888, 30080, 31080 เนื่องจากตอนสร้าง cluster เราได้เปิด port เหล่านี้ไว้
- ซึ่งเรา Expose Internal Port: 30080 ไว้ที่ 2 Node เมื่อ user เข้ามาจะเข้าผ่าน Node IP, Port 30080 ที่ Node ใดก็ได้ตาม IP Node แล้วจะ Forward ไปที่ Service ตาม configure แล้ว Service forward ไปที่ Pods โดยตรงเลย
- เพราะฉะนั้นเราจึงต้อง Expose Node Port: 30080 ตอนที่สร้าง Cluster เพื่อให้ user สามารถที่จะเข้าถึง Node IP ได้แล้ว forward ต่อไป

	| Port ที่เข้าถึง (Host) | Controller                             | Flows                                                  |
	| ---------------------- | -------------------------------------- | ------------------------------------------------------ |
	| 8888                   | k3d Load Balancer / Ingress Controller | เข้าถึงผ่าน Ingress Rule → Service → Pods              |
	| 30080                  | Docker/k3d Port Mapping / kube-proxy   | เข้าถึงผ่าน NodePort 30080 บน Agent 0 → Service → Pods |
	| 31080                  | Docker/k3d Port Mapping / kube-proxy   | เข้าถึงผ่าน NodePort 30080 บน Agent 1 → Service → Pods |


## LoadBalancer

1. สร้าง cluster ใหม่

```bash
$ k3d cluster create my-cluster \
	--servers 1 \
	--agents 3 \
	--port "8888:80@loadbalancer" \
	--port "8889:443@loadbalancer"
```

1. `kubectl apply -f 01-simple-loadbalancer.yaml`

```bash
$ kubectl get service -o wide
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE     SELECTOR
kubernetes       ClusterIP      10.43.0.1       <none>        443/TCP        10m     <none>
my-web-service   LoadBalancer   10.43.210.170   <pending>     80:30080/TCP   8m26s   app=my-web
```

- TYPE: LoadBalancer

# Conclusion

- ถ้าเป็น Internal Service เราใช้ ClusterIP, Stateful App (Headless)
- ถ้าเป็น External Service ใช้ ClusterIP (ผ่าน Ingress), External Load Balancer, NodePort (Setup Cluster ของ K8s เอง แต่ไม่ได้ Setup Ingress ทดลองใช้ใน Dev Env ได้)
