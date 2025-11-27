
# K8s Objects and Components


# Pod


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/af2a4b47c01cc490c388d9ba57165c42.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=ECQSrlyaqZUn%2FLLftuUTLwjNHRjI5rcavifgw2kozM1%2BuXmEbPrutGbGwUHLlRbRrXTeUf%2B8Nd42GDocTmHKzUuc7ZCQPd6KYVzsIJujA4BU72CHB%2FmwFH%2B%2F8eKIayZzkUCwHxxfZnjFeSm%2B8W%2FhLZ%2BHI90LxebGirMB2nC8U5rA5lPniEF5OL16qGlHBUSCXWxMbd8bSj%2BMqAWgE92oUMOtnTvGCcqMx2KjV95OnSKYevjngK02yPtvnvLpAqdwNZTWXwbh1NNHGTwaH5TBcZR6JDQFed1QeHs9%2F2XYZpdL%2Bp9k4hlj%2FzQHsKeq3DwW2a7%2BMNxHKeiudSfsx%2FRgpg%3D%3D)

- Pod คือหน่วยของการ Deploy ที่เล็กที่สุดที่สามารถจัดการได้ใน K8s
- Pod ประกิบด้วย 1 หรือ group ของ container มากกว่า 1 ที่ share storage and network resource และ spec (ข้อกำหนด) วิธีการรัน Container

Pod คือสิ่งที่ Kubernetes จำลอง


## Node VS Pod


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/d2b21d2fbec008aac331b68fe800ddc3.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=dL%2BEcm5Y2wCVB2H1gE%2BwZo0S7NznlwNqNUVYnKqBhmInlqA5ntDdeIhkRH737uRuEvBOpSOdrAuoqKzr%2BGUf6pRvbQzMPutokZzGxnfAkXqnSQRZhkTimdMd2Ub5hTG6dIpyu4UkUksgk26YXZ5wE8ICOel9eIEu%2FCpb6mYz8irx%2F2%2Fjm9H62Wc0Z%2Bg8ovBKXpd8A9RyEIyffdwk2U%2FD5yxXhM5yuE8pIsHZwwV9RJkAyGDkyfAOEM7zHqNCFLK%2B9558vIEp%2FT%2BiuvN%2BZagdz6moxVpKLbp8Sl%2B8nSrAAhPISBcE3GapyQiPXR9V0qqRqT8iKnENMRjVBKmlhgBCFA%3D%3D)


Node เช่น เรามี Server ที่เป็น Physical หรือ VM มาช่วยการทำงานอยู่ใน Cluster ของ Kubernetes

- เช่นเรามี Container ที่เป็น Web Server อยู่ใน Pod ของ K8s และ K8s มี Host machine เป็น Physical Machine

## Storage and Network


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/d732835a1a01434ef1c08c03c70fc879.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=MZFOXl%2F80ip4i%2BSOqDibjeqfKkRPKnCG9j9yT%2BPlwWiVn08P40kq7a20UnGJ8WzeAs542GIBPQIiR%2BHNT4c6LXWsqzkWZGNTHqYlgT9cvI%2FWrirOmpTf4v2OSsuRBez4S2PwJtRmsx2gKJt8c0F32Ufctl4QOzx%2BYXaXyXH7OUy6bL1nJk4Rj5iDNbDLLI3KFUBX%2BXk35Is3oc6%2B%2FWIRTt4lLUVCFeQxTkVB02eXtdJSM%2FsRl2QaxPl7rGwf5v%2FD2hI9pXnAHN8dJtuzknSRndjQgibvZs7N4H72D5ckD7D8vnOShLtvoQG712cP4GwLMFpaN4mj%2FOftEdaR27cFfA%3D%3D)


โดยปกติแล้ว 1 Pod จะมีอยู่ 1 Container

- 1 Main Container และ Site Service Container โดย Site Container คือเอาข้อมูลใน Main Container เช่น Logs เอาไปส่งให้กับ External Services ทำ Observability

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/5b028d421e65f95da1505603523e3e33.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=XMQ0%2B148mJhUH22%2FI%2FQKqO%2FibfagEBVfjyUZVRrrHZyvIfSNtLGw0MqEqxkoTzFwikKqOfMocEEqktXylU8u3ZNIT7mYE33A9qD5%2FCaZlK48CjbyenemxyYkAca779VZp20Y5%2B49YmqAFLzNwnh1Oqh9nMcTeZwM0BBYjU508T4hrmtrBliLhwkNpShGbucRlgmZWYwpxSguxuVjMwGzlcH5E7WkyQsYZUp3NTk7OQzonBMZF%2FqKTn2v5s8N8Q6mDkzDs504njfwDNM58Gk3F%2FUejBs90KFbQcmF04VjQVmxjV6iKEILeES2ukUkAlfWD8UdOvdB3uZ2HWlNKcuEzA%3D%3D)

- Pod สามารถระบุ set ของ Storage Volumes ที่แชร์ใช้งานร่วมกันได้, Container ใน Pod สามารถเข้าถึง Volume ที่แชร์ ใช้ร่วมกันทำให้ Container ภายใน Pod สามารถแบ่งปันข้อมูลกันได้
- แต่ละ Pod จะมี IP Address ของแต่ละกลุ่มที่อยู่ Container ทุกตัวใน Pod จะใช้ Name Space เครือข่ายร่วมกัน รวมถึง IP Address และ network ports
- Containers ที่อยู่ใน Pod สามารถสื่อสารกันโดยใช้ [`localhost`](http://localhost/) , ถ้า Container ต้องการสื่อสารกับ Entities ภายนอก จะต้องจัดการ การสื่อสาร วิธีใช้ share network resources เช่น ports
	- ตัวอย่างการกำหนด Port เช่น ถ้า Main container ใช้ port 8080 แล้ว Site service containers จะไม่สามารถใช้ port 8080 ได้

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/8d4acbeec14dde32591d36dd809876db.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=XdyzziFDRAn03CnX1isayZjjpfDI6VvPAhD6SPfcpl6oV4PpiLmfg%2BIcea4Eyq4IjU1YP%2By90XSgupY14R005S9fpj9%2BGyr3Q7gdhImn6KWDCQpEzY3cVRXNDtwl4pubbpaGxvqV1EP5qtY%2BkeXXpSuagPZbrxdwBsp3ZPxUxVhgWtyOcGcWCGR6s4nuXvNNelnZD4HzukRXFIl6snl36kALe0FL8XXx782zxunrggjjcE0xsEhasl1Ewzd7zl0Lj9uGNjsMWh55ttMogE2uMUZHkUbJI6PiqLxQlmqWynK9qHEYgVBTTnbmyaBNRHbQxC9dHLWpG%2F8oPRtHaOVWbw%3D%3D)


Flow

1. เรามี Application, Database ต้องทำให้อยู่ในรูปแบบของ Container Image
2. เสร็จแล้วนำไป Deploy เป็น Container ที่อยู่ใน Pod ให้ K8s manage
3. K8s จะดูว่า Physical server ตัวไหนว่างก็จะนำเอา container เข้าไปใน Pod แล้วนำไป start, จะดูว่า Node (physical server) ตัวไหนว่างอยู่

## New IP on Re-create


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/4ee0a3dfa5c316a84d6b1745e67cd529.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=BBvf4CvQZBGCLnX%2Byk%2BxgLw9ZuW8hGWCdygdozMAPcOUDmsF4MX2f2udWPoQOP9F5cIfsGktmxJ6UY9Vy7q1WJwU%2Bo5Hl0JxCwR1qie9jDLOfb2baqLZlc48HN%2Bgu2RH3cGmjgEIgHEQTSrOvzai3hdXbgmvwQcqnyrGry5YsAyyRQtm4gyKlEZm%2FlvpIhz5LtuDLvB0drd1fz4c2KWsqZStdjYHQXKAEf3VbT0MdP3yF41QnLjPa9ewTuDz7xmQDyAxrTU6CK7tgwwG0AJ00eI%2F7FG6wz1XbGrfHDfNBSyjiLpCOkKnxn5VGl0VJNZw6r6FznRVAxEOFDIhmk5iMg%3D%3D)

- สมมุติว่า Application ของเราเชื่อมกับ Database Container อีก Pod อยู่แต่ว่า Container นั้น down หรือ died ไป k8s จะสร้าง Container นั้นใหม่ขึ้นมา แต่! ถ้ามันไม่สามารถสร้างหรือ restart container ใหม่ขึ้นมาได้มันจะทำการสร้าง Pod ใหม่ขึ้นมา ซึ่งจะเป็น IP Address ใหม่
- Application ที่เคยต่อไปที่ Container ก็จะใช้ไม่ได้, k8s จะมี object ชื่อว่า “Service” ที่เอามาใช้ช่วยแก้ปัญหานี้

# Services


Service เป็น k9s object ทำหน้าที่เป็น (Stable Entry Point) สำหรับกลุ่มของ Pods ที่ทำงานเดียวกัน, Pods ใน k8s มีอายุที่สั้นและ IP Address เปลี่ยนแปลงได้ตลอดเวลา Service eจะเข้ามาแก้ไขโดยการให้ IP Address และ DNS Name ที่ Stable (คงที่) ทำให้ Pod อื่นๆ สามารถเรียกใช้งานกลุ่ม Pod ที่อยู่เบื้องหลัง Service นั้นได้

- ทุกๆ services ที่สร้างขึ้นมาใน Kubernetes จะได้ IP Address ออกมาอยู่แล้ว และมี DNS ที่ทำให่สามารถใช้ Host name ของแต่ละ Container ติดต่อสื่อสารกันได้
- ตัวอย่าง: หากมีการจะ Deploy service: my-app-service, db-service จะมีการสร้าง Service name, IP Address
	- Service: my-app-service

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/98e7d10356bb15bb802942a68e238462.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=HDv4GwbkAW%2Bi%2BEgOQaGVcn5zdgVyH6X0aY88PM1A2MpyAGP%2Bxx83O7V8H8RUDsUOUf9qSaT1W5hk48KNE9q63YM3x9Bs%2FBiti016c0P%2Bs7%2B%2FBMsrauwC5wqxTG%2F1Qi%2F2ZfQO2DPSBvnf%2BOeE24hIRp%2BaO5OTyUM%2FXzHpBtxU6zX9EvRCOzqeeBijj3Yl%2BXgMwwG1JTu2ArHSlXj%2BZwCvRvAkS8wwP3KfE2U%2BIP9jIM3zAJXWK8NNKf0e4WB8e1FF9JfLVTsS14mR6XPCUczw6R4RcC1vvPv8gBhEsdD3Lj40RJv1VTz0vgCP0aEh9W65gvdInJ9rR4l%2F4hO1PXSWaw%3D%3D)

	- Service: db-service

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/225b42edcce4fec7f48beb0bd05d6770.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=cRXQS4pYByzkYzPA01H%2F79c4x9Wy2ujs%2FHRPaRJqH1x28jMJnpPldcmBi4pJBgIBBwTygY2GmSI1xICGYNmsjE4aN62wqVK47Nx0elc3moBFYYkx%2FevqTBhbq198Mzzlexw5Tmj0sDv%2BZvXwsqht9uW083abmtvJBCaZYSd9gxJSqKSHMOSssRKcAm2zZc%2BCC9NoSA0%2FvWAFiAibJ8AGxSN4MZJuHZDkXd3%2BWK6SddKBu5xJmh1P12xdXv8BuCHF8IYyvDYbghGkPYHSSZRbmYXoEECuFZLIriB8xIVuVTu5M2Zny7SgHosHgGRNiSiSiIemLe657oiQWGsUZqPKgg%3D%3D)


Scenario: Application ต้องการติดต่อกับ Database

1. Application จะ Connect ไปที่ Database services แล้ว Database service ที่เป็น IP address และจะตามหาว่าใน K8s cluster มี Pod ไหนที่เป็น Database services บ้าง
2. จากนั้นจะ forward traffic จาก my-app-service ไปที่ Pod ของ db-service
3. ถ้า Pod ที่เป็น Database service died, k8s cluster จะสร้าง Pod ใหม่ขึ้นมาพร้อมกับ Assign IP Address, Pod ตัวใหม่ที่ถูกสร้างขึ้นมาจะถูก registered เข้าไปที่ Database service, จะทำให้ Database service รุ้ว่ามี Pod ใหม่แล้ว และแทบจะไม่มี Downtime

## Logical Views with Node and Service


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/027ffc363e6d4274dc26c51d28063fd2.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=JXZvmyTLLG7ttH%2FPo%2BR726x2Y8T0M%2FtKbsMc6s6vsUsty6Ub2kYe453uh2dj4Kzr7GsHkcP6UftdRsGyFQD%2BnwPgF1NHq4U2k8NtNX7WTnMnKOMdTGuhnIOO3oQ7gnUWaX%2B1UT6weGCn4ZSRD%2FY42aj5ZRxKaIauGrTzvKRq%2BJbZmb1U3p5z3gYg5lqCybBJRbbdbCstXwrfTqBKC%2Bz7XeFajtQvLglNcAr1r7BgLV2al%2Fdsih1iCsAxP6hkANU39eXW6FhCQ3KdzK0ZaMP8b56vScpk8YpiiLEG6irN0pefpifyxax0XdCA5dDnlx6Bgl%2Fg4XCwquLBPKijXq%2F1bg%3D%3D)

- Namespace ที่เราพูดถึงสามารถ Access ได้จากที่ไหนบ้าง?
	- Internal Service: User ภายนอกไม่ควรจะเข้าถึง Sensitive service ได้ เช่น Database service
	- External Service: การเชื่อมต่อผ่าน External Service เชื่อมต่อผ่าน → Domain name, IP Address ในกรณีทีของ IP Address, Customer User ก็ไม่ควรจะเข้าผ่าน IP Address ควรเข้าผ่าน URL แทน ซึ่งจะต้องใช้ Object อีก 1 ตัวแทนที่ทำหน้าที่ Mapping Domain name คือ “INGRESS”

	# Ingress


	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/dbb321d1784d560f6319e66e2489fc22.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=XAn2E1p8QIKg5MAlUjWWEWasFcVJgyTjfTcZeQkzaceo5eLHhYsgEJA2mUXg9QaXWykHqesNrirrkNNzFmkgn0zRamPK0ngAudCaSxHRCTBRbyTDMV5OwWqFgR3GTBY48duSSvV8x69C1fGl5ppfaAeTlGBKqfQhmO0sQQxaIWovap0xiLuoVBpX05%2F%2B4RozAflKTcPtHwTrMTu5W8eUFDRjmulHMvJ17aASn4VrkteqmaTzIg7HUnW3b6%2BjSXfGAmCoCk16iWdAYctfZDT1lXQ55AyRWSHwkcn0G7e7WQSccERUNp8VCRjYo6wx%2Bc6Ylya6%2F%2FXkG%2FiOF1HnWgus%2Bg%3D%3D)


	Ingress เป็น API object เปิดให้ External access เข้ามาใน Service Cluster ผ่าน HTTP, Ingress จะ forward traffic ไป service ที่ user ต้องการ

	- เช่น client เข้าถึง my-app-service, ingress จะ forward ไปหา service ที่ user ต้องการ
	- Example: Connect External Service
		- มี object ที่สามารถ forward request จาก internal service ไปยัง external service ได้ เช่น External Database,
		- ตามรูป Service: db-service ได้รับ request จาก Application container เสร็จแล้วจาก forward ไปที่ External Service, External Database service ซึ่งหาก DB Service เปลี่ยน IP Address ก็จะเปลี่ยนแค่ configure ใน Service: db-service , main application ก็จะไม่มีผลกระทบ
