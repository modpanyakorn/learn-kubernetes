
# Nodes


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/80412ed1a95c6bbf8912d6f19e83a8f8.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=OLSyZlhdlE4JTctFlRgwEeclty7li7zUgkyI%2BreA%2F7C8QXPypZf11tUlQ6HyUBlYKaBdDdBgab3InIvWfKej0F0d3qWNt0%2FR%2FHyySdFCg6wSF72%2BK9eMMRX0aovoPICtsTC2S%2BTkb5fROpUPEbrLkRpMLRU9v%2BBMGa%2BxFjpQxxa7161LO0eKArp7z%2FcwEEDsoUgpaBYD4ROuHVzoI0YH6hYN7LvrdsnZCbkdige%2BEvj2mucCFZTQ4F%2Bc02VxumpsbrKxcEFP6yFD90zo26fWAWKgkctV%2FuPbiriDNcJedmNmCrpPPLpAGaPNSHUYyL0KNcIfw3W8%2BQ52Cb2wWkOy%2BA%3D%3D)


[https://www.ais.com/an-overview-of-kubernetes-architecture-and-container-deployment/](https://www.ais.com/an-overview-of-kubernetes-architecture-and-container-deployment/)


Node ที่อยู่ใน K8s จะมี Worker Node, Master Node (Control Plan Components เครื่องที่ควบคุม)


# Worker Node


Node Components สามารถรันได้ทุก node, maintain pods ที่กำลังรันอยู่ เตรียม k8s runtime environment ให้ Pod เหล่านั้น ให้ทำงานภายใน Cluster ได้

- Node ใดก็ตามที่ติดตั้ง Node Component จะกลายเป็น Worker node

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/8e94d5cbb3276de960687db0f6a099d7.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=d9N78wIaF0b48%2FLvAePl8KztJ%2FbUbBsZjBudSc5RHutGWz6M77PgnxHXoW8UEq8rE2V789yrtQQM2u%2BHEnt8zNaxQWhndK6rtFGl613n%2BJP4ITrVP6r63CVvdIINoCQC6OvjB9UrhZlAt4PI00dW6miiwumw5pkSTNhM2p7OdujMTeBS4GrwAf0oPe%2BUlzno4%2B51u3fU0Bh9aTN1u95Zb%2Byp%2FsHvCdjDWYfRe3AjDhMXOYpZWvuCwcLjgkvnUygHIL4Erk54IYuWMewcpjKDfQcqH1m6PYqk7%2FtVzZent0t7hCeATR3RRTPz6WkAZnJs9BC3OFoqKwXSLTj%2BdZID8A%3D%3D)

- 1 Worker Node สามารถรัน Pod ได้มากกว่า 1 Pod (Node = Physical Hosts, Pod = Logical Hosts) สามารถที่จะสร้าง Pod เท่าไรก็ได้ขึ้นอยู่กับ Resource
- หน้าที่ของ Worker Nodeคือทำ Actual Work ทำงานตาม Application ที่อยู่ใน Pod

## Worker Node ประกอบด้วย 3 process ที่ install และ running อยู่เสมอ


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/9db4391947fff15315f432c08fdfb5a9.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=FNz8n0EbDK17SuXyyuzuVHjJW0LIO1G3fKWnX5%2BRDqQqeriEh6x%2FhOApweWGAGU7QNUmMyPN13fN3W7RgEVsIcMOVhyq%2BtkEfaSsYIP9jtRF4CnDNGQHOe2PV5Yh3E0%2FBH8LrrCBsxT4El2N53QeF49WrJTdDiDrTZJ32iWQ91EAkmd1PO9H8eRG0FUS69W06C41VOUNAvDCRjQtT%2FKMHPjdZSTotA38nxmdagmhI4Y2JhIuHliAur6YhZrdG78hQY0VW1Er9jo2%2BCrWhhN3kzSP%2FfCBy%2B%2FomDQaTVlIkhIpdam6%2BOgHUXIY%2FcggmXxpEeKf8NBtFjMi5J6vpUiM2A%3D%3D)


# Container Runtime Component


(Container Runtime) Kubernetes ต้องสามารถทำงานโดยการบริหารจัดการ Container, Component แรกที่อยู่ใน Node Component ของ Worker Node คือ “Container Runtime”

	1. K8s support Container runtime หลายตัวเช่น Docker, ContainerD, CRI-O (Container Runtime ไหนที่ Implement “K8s Container Runtime Interface”)
	2. ซึ่งในปี 2022, k8s ตัดสินใจลบโค้ดของ dockershim (Agent บน Node เพื่อแปลงคำสั่ง CRI ของ K8s ให้เป็นคำสั่ง Docker Engine เข้าใจ)

# KUBELET Component


(KUBELET Agent) เป็น Agent ที่ต้องรันในทุกๆ Node ที่อยู่ใน Cluster มันทำให้เรามั่นใจว่า Container กำลัง Running ใน Pod, kubelet ใช้ชุดของ PodSpecs ที่ได้รับมาผ่านกลไกต่างๆ และรับรองว่า Container ที่เขียนไว้ใน PodSpecs เหล่านั้นกำลังทำงาน และอยู่มในสภาพดี kubelet จะไม่จัดการ Container ที่ไม่ได้ถูกสร้างด้วย Kubernetes


	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/41e41134474e49e194a3c5a2e343aa5f.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=N9dxMgFUoHZqjlzf6wzrjY%2BctsGMw7u37D%2B2NFEjJJDZ3KlAKs0UK64deZskOXIBTE3VVe4WVIlhoNCv99F8PMwPn5Ya4LUJ%2BG%2FIGJFDFQlxeiUfmmXFh%2Bpze6LYq4RCtwcIZUSSDXdBTuzpv4ggrJv1GUz5oUjmAo14m6FY%2FC4l2ITGAQqMk%2Fx1Vn5gSRNIteY4TlCn0CooiGuNWPUDlkXO1pwBVGp0ifw6WkXohOgZViZIJZDcJcaGO2oAkpRMsn0pofCMdyeTso1ybi1NDWOC7umyr%2BlbFVWjTPZ9gtgQyzk4rIPDKDV2JKzd%2FIX4go%2BmsxOSYnzG0PC%2BwpOaUw%3D%3D)

	- หน้าที่ของ PodSpecs คือจะบอกว่ามีคนส่ง Spec ของ Pod มาให้ ว่าเครื่องๆ นั้นต้องมี Pod อะไรบ้าง kubelet จะต้องสร้าง Pod โดยทำงานร่วมกับ Container Runtime, หลังจาก kubelet สร้าง Pod ตัวของ Container Runtime จะนำ Container ที่ได้สร้างไว้ไปใส่ใน Pod
	- kubelet สามารถที่จะตรวจสอบ Pod Running ว่าเครื่องที่ตัวเองรันอยู่เป็นยังไง Pod Health เป็นยังไงบ่าง สั่งรัน Container, Container Health Check และสามารถ communicate กับ Master Node ว่าสถานการณ์ของ Pod ตนเองตอนนี้เป็นยังไงบ้าง

# KUBE-PROXY Component


kube-proxy คือ network proxy ที่รันอยู่ในแต่ละ node cluster เป็นส่วนนึงในการ implement ของ K8s service concept, kube-proxy maintain network rules อนุญาติให้ Network สื่อสารไปยัง Pods จาก Network sessioins inside หรือ outside ของ cluster


kube-proxy ใช้ OS packet การกรอง packet ของ OS หากมีและะร้อมใช้งาน ถ้าไม่มี kube-proxy จะ forward traffic ด้วยตัวเอง

- kube-proxy (Network Proxy) รันอยู่ในทุกๆ Node ทำหน้าที่ Implement Service (Service = object k8s, logical ip address, logical name ใช้สื่อสารกันระหว้่าง Service-to-service, Pod-to-pod) ซึ่งเบื้องหลังมันคือ kube-proxy

## Scenario


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/4ea03dd0cac27996403160f197a6f036.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=fr1U%2B9nN02g2l7%2BOVbEVs0EVfTcRkIrwc5EOXJj7sJ5xbqv%2BT6LQR8ltoFVZ3PmVhtjYtevCphyV4kGr8DbJSfhj5s%2BhlxuqqZRi8RgFjcr5bkUMA58ic%2BVR4MgzFtJdrWD%2BKYBSxG%2FLcJUhEuuHclV9zQNHyTq9Q3JootBzAKJupNI5hAATuChD4%2BTFIZeruIluFteOpQdfPuCZ%2FpQOde%2FW5CiTVg66wzsWBJ2fbwMHp9DF8vNKnwaQrFSz3yx%2B2f7N%2FtQ%2Fsr%2FKyKYysYedbZhEBJelE9hEADn5Odtk66nrTW9BwTWp2emr8wgMSNuO9GdAkB%2BcynyS3hqav%2FlxQA%3D%3D)


Application ต้องการ connect กับ DB Service, มันก็จะสื่อสารกันผ่าน kube-proxy, kube-proxy จะเห็นว่ามี Pod ไหนทำหน้าที่เป็น Database service แล้ว forward traffic ไปยัง Node นั้น

	- แต่ถ้ามี Pod ไหนที่เป็น DB Service แล้ว App service อยู่ใน Node เดียวกันมันจะไม่ forward ข้าม node ดังรูป
	- หรือ ถ้า Node ตัวเองไม่มี kube-proxy ก็จะทำหน้าที่หา DB Service นั้น

สรุป Components 3 อันแรก

1. kube-let ตรวจสอบ Health Check Pod, Container
2. Container Runtime สร้าง Container
3. kube-proxy สื่อสารกันระหว่าง Service-to-Service, Pod-to-Pod, ServiceA-to-Service-B, Pod-B-to-Pod-A

---


# CONTROL PLANE COMPONENT - MASTER NODES


เครื่องไหนที่มีการติดตั้ง Control Plane Component เครื่องนั้นจะกลายเป็น Master Node

- Control plane component ทำหน้าที่ตัดสินใจเกี่ยวกับ Cluster โยรวม เช่น Scheduling มีการตรวจจับ และการตอบสนองต่อ Cluster Event เช่น การเพิ่ม Pod ใหม่ เมื่อ deployment replicas ไม่พอใจ หรือ ไม่ดีพอ
- Control Plan Component สามารถรันได้บนเครื่องใดก็ได้ใน Cluster  อย่างไรก็ตาม เพื่อความง่าย Setup Script จะ setup control plane component ทั้งหมดบนเครื่องเดียวกัน และจะไม่รัน Container ของผู้ใช้บนเครื่องนั้น
- Master Node ทำหน้าที่ตัดสินใจภาพรวมของ Architecture ว่าจะเอา Pod ใหม่ไปรันที่ไหน หรือ เอา Pod เดิมไปสร้างไว้ที่ไหน ตรวจดูว่า Pod ยังรันปกติอยู่รึปป่าว? ให้รองรับ Load และตามสเปคที่ระบุได้ ถ้ามีการเปลี่ยนแปลง Configure บางอย่างของ Application ตัวของ control-plane จะตรวจสอบว่าถ้าไม่ตรงกัน control plane จะ implement ให้ตรงตาม configure ใหม่

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/bd0ecb4accc45fce461a9c6f4a4015b6.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=dPeBr4Xbjs8%2BzxZiUQio%2Bka%2BF3kJbwCNFWbL%2FkjAotoq8QxoPe03MIF2wgghD99ct2QBe%2FWsJO6p2NAIVzTRC6mB1g99FGo2ydheX396xCoBuqm73F%2BUk%2BnaGaTMgxP61F%2B3HCw%2FCieNfOGxOyfF5Rwj7WDR2vKJ%2BtKg3JA8P7ZB50NA1r1WIfchgBvazofowY%2F8AgL92wIoR8ZQhcAGZZI2TVxCgpsNc%2FUgdgHcX38KofIrteTuwBbTcYea1YDp2rMZN%2B9ctnDAdRRMUZKBaO2jS7LnUQJVXcoGNSW78vb0HBE38wBtIMTJqHVYDiIVnUFHEpMnL4%2BKXumDCyh6zw%3D%3D)

- Control Plane จะ Interact กับ Admin ที่เข้ามาจัดการ Cluster, Interact กับ Node ผ่านตัวของ kubelet, kube-proxy

## Master Node - KUBE-APISERVER Component


API Server เป็นส่วนหน้า (Cluster Gateway) สำหรับการควบคุม Kubernetes การใช้งานหลักของ K8s API คือ kube-apiserver


kube-apiserver ออกแบบมาเพื่อ horizontally scale ปรับขนาดโดยการใช้ Instance เพิ่ม สามารถที่จะรัน kube-apiserver หลาย Instances ได้ และปรับ Balances การรับส่งข้อมูบระหว่าง Instance เหล่านั้นได้

- หน้าที่ของ API Server เป็น Gateway สามารถ Authenticate ได้ด้วย Gatekeeper
- กรณีที่มี Request เข้ามา เข้ามาผ่าน API Server เสมอ และ Validate ตัว Request ว่าถูกต้องไหม แล้ว Execute จากนั้นส่งคำสั่งทีได้มาให้อีก Process นึงที่อยู่ใน Master Node เพื่อไป manage Pod, Node อื่นๆ, Container หรือ Container ใน Worker Node อื่นๆ

## MASTER NODE - KUBE - SCHEDULER


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/42fdd74835633a665a553eddf6419738.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=hlI3B2RmLfXUfq7NAgfOyHZRoIQi%2Fx2qpu7ojUsMEy6mpfTU1iM3VtL4FR9C%2BNOI3HugsFLi1fPNAmzZePso7CFb06sBh6cG6RJ8bTdbMTf%2BGOpQeXmpJr%2B5nfslbPJY6i2Y5qmo6q90VWlQnbhjmTtsI6vLecqZyqR8orBrtMbJmxtMMMygmIGof3nASAPsVzLwxfY9mGuBQ2AuqpmOhK65YQLgJu9O7uY2kQh9ZY0ZqIaKz9ztntpMzEi%2Fi4aKywIRmj1Sl%2FbzxpIFghkykOt8p5f5TVOxv2A7GdwUltQQ7MWLOH6y2ScI08%2Bm%2Bw7F5qEUhfagGm7YQ3RnbLlvLA%3D%3D)

- จาก APISERVER Component ถ้ามี Request เข้ามาจะส่งให้ Other Process นั่นคือส่งให้ KUBE-SCHEDULER
- KUBE-SCHEDULER คอยดูแล Pod ที่สร้างขึ้นมาใหม่แล้วคอยดูว่า Pod นั้นจะถูก Assign ไปที่ Node ไหน
- ดูว่า Resource ที่มีเพียงพอหรือไม่ ซึ่งมี Policy + Algorithm เข้ามาตัดสินใจ Policy เช่นเมื่อมี Pod ใหม่ให้ไปวางไว้ที่ Node3 เพราะเป็น Node perform with GPU นี่คือตัวอย่าง Policy
- Scheduler จะทำหน้าที่เหล่านี้แล้วไปประมวลผลผ่าน Algorithm

## MASTER NODE - KUBE-CONTROLLER-MANAGER


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/2069ff1060c5796afe51e731d3e10056.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=NvKkE4HMA%2BRqFf4NhiAOdkoKpMptGc9ILR841chppfB370aEjCodUN%2BDs3TxJA5kvnYyKujEzOgIvpI%2BDAccgIIew1pTugvsqf4h1ZKFuZI9tX9czWRM3CtN3Gco0oEX0ih%2FUlroxDEq4ttRFTPjXQyhnLtThrCz0F7262Z1jrGOk5XCOpJBkCa4kD31ILKe55ugkcpkE5W3cTF70IzDMyB26qidWDnFyfwSh9iGH9h053LpsKFmk0aNox4aK1GO9Hd0eBxInIPYNLTPPjXMGoAayJbqbMPrg%2BNNMSimr0%2FyuOT%2B6RHVle%2FyiAbLliHOb70O4atpigiFB7IVsHCNDw%3D%3D)


เป็น Controller Process (รันเป็น Process) แล้วแต่ว่ากำลัง Process เรื่องอะไรมี

- Node Controller Manager
- Pod Controller Manager

แต่หน้าที่จะคล้ายกันคือ ถ้ามีอะไรผิดปกติในบริเวณที่ดูแลอยู่ เช่น


มี Pod ที่ทำงานอยู่ Died หรือ กำลัง down หน้าที่ของ KUBE-CONTROLLER-MANAGER คือส่งปัญหาให้ API Server และส่งให้ SCHEDULER ทำงาน (Assign New Pod) ไปให้ Node ดูว่า node ไหนว่างอยู่, โดย Scheduler ทำการส่ง Spec ให้ kubelet, จากนั้น kubelet คุยกับ Container Runtime แล้วสั่งรัน Container ใน Pod นั้นๆ


### MASTER NODE - KUBE-CONTROLLER-MANAGER มีหลายแบบ

- Node controller: รับผิดชอบแจ้งเตือนเมื่อ node ทำงาน หรือ กำลัง down (จัดการเรื่องเกี่ยวกับ Node ถ้า node ไหน down จะ Noti ไปให้ API Server)
- Job controller: คอยดู job object ที่แสดง Task จากนั้นจะสร้าง Pod เพื่อรัน Task เหล่านั้นจนเสร็จ
- Endpoints controller: เพิ่ม Endpoints object (เพิ่มเข้าไปใน Services and Pods) ดูแลเรื่อง (Services, Pods)
- Service Account and Token controller: สร้าง Default account และ tokens access api สำหรับ namespaces ใหม่ (Service Account, Tokens controller) เมื่อสร้าง Namespace จะจัดการสิ่งเหล่านี้ให้

## MASTER NODE - ETCD


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/9708a519a33ffc819398af93b5a48e9a.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=mzGWUKcR6srCsTiBrWxd07xsDr6SYFaHOK1kTQ8Ul73AkVZmyefy8qaTfj87t2nBTVdpsnO5CRHpIrO89Om%2F95kbv%2BMc1xxZQgkeIfcE4viLdtltwo7%2FuPXbAQ00ZpYeRzfrWB8PBqp%2F%2FAAe4H5L%2FKUBVMP38cReCR33c%2Bk7JWXoIFmsZ4m%2FPkKhS7%2FfM5r6RRUUlRHBGdfeqYNPIIBBz2hEbxdc9jFR70wYzjfBB3nbWk5twwHg4YYdc2e6cMeeLqupP%2FXkDZlMauBlRnCRAXefQmUB2LHkRQ34ygGHJFtyiL%2F3KrvVboUZ3vusrNCuR931ir2oYGFfRmqnfCRtCg%3D%3D)


ETCD คือที่เก็บ Data ทั้งหมดของ Kubernetes เป็น Data ของตัว K8s Cluster

- มีกี่ Node
- กี่ Pods
- สถานะ Pod เป้นยังไงตอนนี้
- เก็บว่าตอนนี้ Node ว่างหรือยัง ว่างเท่าไร เพื่อให้เป็น Information ให้ API, Scheduler, Controller manager เพื่อดูว่าสถานะของเครื่องเป็นยังไง เทียบกับปัจจุบัน
- เมื่อไรที่มีการเปลี่ยนสถานะใน master node, worker node, pod จะต้องเอากลับมาอัพเดตใน ETCD เป็นแกนหลักของ Cluster ข้อมูลทุกอย่าง, configuration เก็บอยู่ที่นี่
- เก็บอยู่ในรูปแบบของ Key:Value เก็บ Cluster State Change, Cluster Health, is resource avaliable พอจะให้ Pod ใหม่เกิดขึ้นได้รึป่าว
- (ไม่ได้เก็บ Application)

## MASTER NODE - REDUNDANCY


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/65778bd0d38d9b2d30de8292b3a78fe3.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=C0PnI4d8k%2BATz3QvL7%2BjEWI%2F2gKvodnPRsBkgnsECSuSr%2BCksJ%2FP3EA0vSmvqZJ%2BTsgw%2FPZQI%2BoLCG5v4VLOvqmb1Qnn%2F%2FgvG%2FNLgixfNZQXGNMZZC9OgdZQoiY4MGfNe69heRnyRgboBiXYRVldRQXfVgJBVEIpoBo3bV2Q33DrT8HL3h4ATvIN72TAzguQY%2BUc0ZoSDNV5H7FtaqDSLvy5mbnsT34G%2FC%2Byjn4gBjiFqvCC9H7a5%2F2vew0XDYv%2Fu8ceGkeeTvfRCNtreFGbzRBT6XxDRxb8RJp30ZNcCna1z0dvfM%2BstmzwDS2h1X58Ejlvhl%2BiwUsSDhFaBq5tww%3D%3D)


สมมุติว่ามี Master Node #1 เราสามารถสร้าง Redundancy ขึ้นมาได้ของ Node #1


ถ้าเราสร้าง Master Node หลายๆ ตัว ETCD จะถูก Distributed ไปทุกๆ master node เพื่อให้ข้อมูลต่างๆ เหมือนกัน


Example: Cluster Setup

- Master Node สั่งงาน ใช้ Resource ต่ำ
- Worker Node รันงาน, Application Workload, Transaction Business ใช้ Resource สูง, Worker Node จะต้อง Replicate ได้

Master Node ส่วนใหญ่จะมี 3 Node, Worker Node จะมีตาม Workload ของ Application ตาม Resource

- K8s embed logic เฉพาะ Cloud เป็นตัวจัดการควบคุม Cloud ช่วยให้สามารถจัดการ Cluster ของเรา และ connect กับ Cloud API Provider ได้ และแยกส่วนประกอบที่ communicate กับ cloud
- cloud-controller-manager จะรันเฉพาะสำหรับ Cloud Provider เท่านั้นหากใช้งาน K8s แบบ local หรือ ไม่ใช่ Cloud, Cluster จะไม่มีการใช้หรือจัดการ cloud-controller-manager
- เหมือนกับ kube-controller-manager, cloud-controller-manager จะรวม Loop ควบคุมหลาย Loop ที่เป้น independent ต่อกัน ให้เป็น Binary เดียวที่เราสามารถรันเป็น Single Process ได้ สามารถที่จะทำ Horizontal Scaling ได้ เพื่อ Improve Performance
- controller ต่อไปนี้สามารถใช้กับ Cloud Provider ได้ในการทำงานหรือ checking ต่างๆ ใน Cloud
	- Node controller: Check ว่า Cloud Provider มี Node ที่ยังเหลืออยู่หรือป่าวหลังจากหยุดทำงาน
	- Route controller: สำหรับ setup routes ใน Cloud Infrastructure
	- Service controller: สร้าง, อัพเดต, ลบ load balancer ของ cloud provider

# Overview of K8s Architecture


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/305e0bd6346f8d134ff8ea3731dd5447.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=ZIHYVoXpHEmBBBxFAISNc5S8ekEv8VkdBQK8XLXBEDoK52x0sgpHB%2FAIJ72U0sh6bTMTHw76lqhjHj1DIcW1VEnGZQsOfcLS1jrVCBuz7SQ70qFhDU%2BArovCZGESWqYjYvm1xUlqr8TBo6yV3inerVsFL4GT%2BxcnDZIciDGIv7lMaoCYCYzC6ty%2BKk6d%2FHAasPdVsSIVphtVM%2FVcQYKSfMWfXiERoYaEQTeohJ8eBXDd0xhkN2v4gSG1gWyoVGke%2BoxJgvmN4fViZ31jHy4B96KhujhIpDlW6hMrTmwuZaJw7jRUEOeHMIe%2FL32zZlsMAsXlPQTfZRrfY2YWKiFPuA%3D%3D)

- Node ไหนที่รัน Control Plane เครื่องนั้นก็จะกลายเป็น Master Node
