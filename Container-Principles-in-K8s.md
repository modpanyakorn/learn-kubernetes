
# 1. Single Concern Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/fe3f9882c850954d839779a0dac5b696.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=d4A6qBpzYPfVRlYCbi8FgeMIltfvvUwo1GUP8HLeGg%2Bep0acPIKXnSrbkBixVrkBVVtUVVZgOT2utsBvDkfwGEA6%2Bw78vFlsbcxu2LU7nVkwocEnmilINZxxCl7nbYHrQV2e%2F2cUV5vdrUkhk23DrBhCGQDwXmKjiU4tfP%2B7in7SKXlkklgleQkN4x6J17pJ4pJe%2Fch0V1z%2BIZrxlKeCVxfY19NNuhhox%2F4HLZ0ufteCVR614gWAnsoQh1ILmI7TSjQqFDNBWtIU9T1ShrVlW2DpGesxWRfABCKO%2B3D%2Bp3xd58AsXQ%2BZ4N2XVMAhjZrYvdWHQdRaOFXZNd28T9W3Mw%3D%3D)


Single Concern Principle หลักการความรับผิดชอบเดียว

- แต่ละ Container ควรมีความรับผิดชอบเดียว Single Concern และทำสิ่งนั้นให้ดีที่สุด
- Concern ในที่นี้หมายถึงชุดฟังก์ชันการทำงานเฉพาะอย่างของแอปพลิเคชัน
- หลักการนี้แนะนำให้ รันฟังก์ชันแต่ละอย่างของแอปพลิเคชันใน Container ที่แยกจากกัน
	- ง่ายต่อการ Manage, แก้ไข, Scaling
- สัมพันธ์กับ K8s
- Best Practice ใน Kubernetes คือการใช้ 1 Pod ต่อ 1 Service เพื่อให้ Pod นั้นทำหน้าที่ตามหลัก Single Concern Principle

# 2. Self-Containment Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/1d72068899e6ad7dfefeaef857e943af.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=RCSAuRzYCzd84Hzq08B9IwW7DpFW9XMehVf2C6vdL7S2%2BIfsh2AMId9jGp5Ei8YdENfwC4hrjrOrRIfJr5Pmsxo5P%2F67PF9AmmyclvAyw8pHErDtnFQ5R%2BwJagLxGhlsF3DE9xGIng1QvWgDl0Lp4t4CMOqPosP5me66CfKxuD%2BziNTWqBcEfxRYy4Oc4f5eNhqSYZWhGZX8cCi4I0vgXCSVgBIXrK7HE59o5AtAFHzzVKXhzDptmEdIsuta0hRLA0JRc2%2B43bApxZYirkWfr9cuL31WD1I4ku6UndsVDSNDj%2FI0WLTmHS098MFB7DaNdBGeiCW%2ByYiilKrmxoRAiA%3D%3D)


Container image ควรมีทุก dependency ที่จำเป็นตอน build-time ยกเว้น config, secret อื่นๆ ในช่วงที่ใช้ใน Run Time เพื่อให้ container ไม่พึ่ง environment เฉพาะเจาะจง เช่น ไม่ควรพึ่งพา Libraries หรือ Dependencies ที่ติดตั้งอยู่บน Host Machine (Worker Node)

- Container Image ที่สร้างขึ้นแล้ว ไม่ควรเปลี่ยนแปลงระหว่าง Environment เพื่อให้การ Deploy ทุกที่สามารถทำซ้ำได้
- Configuration ไม่ควรถูก Build ทั้งหมดเข้าไปตั้งแตช่วง Build Time เพราะจะทำให้การแยก Environment ทำได้ยากขึ้น
- ใน K8s ก็จะมี object ที่นำมาใช้ในการเก็บ Configuration ต่างๆ
	- ConfigMap, Secret เพื่อส่งค่าเข้าไปใน Container ช่วง Run time

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a377eba41b36cc6a323a0739deb7b1f4.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=dR9CtGCd3KKDBrxlKrha1lIjJxHC7xy8W4ADinP3uQKWVh86jFEEQQ6VErkIbvKgAp09Vzy6vG8fhVFbGjn3gJVMeU4kT5NmSPmfLO6NyfTGH3XhDJ00iHqATZPqSiaDv9JhEwMf9TcmPJbJp3EJVOTFHOZmgDWVvRITKbH6o%2FRKT8hm6%2BiaHYNDHXXK5z5%2Bm1jneL29UJDzffPm2eTmx2rSOa%2FHv1aLbsWyyyO%2FLEA320jDBn6EPqhiPY87AJSKCAWtnaIBZk2Ja%2B5OafyJy3WQQ%2FtxDvLiNplpv6YWWPBljoxi3nHpJbPDg76xzhZw9vDCi4FRyHeB3yVE8OfHDA%3D%3D)

	- Storage ใช้ Volumes เช่น PersistentVolumeClaim ในการเก็บ data ภายนอก Container โดยที่ถ้า Container ถูกลบออก, down, crash ก็ยังคงอยู่

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/413300314fb8281129f8453626de7a02.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=mdHId93fQHQGf58ZVriQyhEl%2Fo3xUvsnt2hDLqAKUdW8BnNHx9YRiXN6C69y2%2FPBqx7nTwx3hWMi%2B8qwRVHPhChIYUDJ6hG1t66%2Fzt71VvEYiAWChXG3IezazzPdZO6pqCDqPIRaU0TV1auETfd8GefvnJyBFYzXhYhvVb0cYFsl8kcBtuJgRba6J7EWN%2Bk8H1v4zPOnWgFo9iGba0UW2m%2BBfW3OT6%2BQKB6jVRTKHtBU7oMVga2J9UoNNbBb0mRE8api2hGhZVvPWdTUaXZMieiv1iUUTyrOpc0CtXznfA3nxxaY3iqE%2FkTOBHjOvF11BDTpAUqjYXqH3NkuKFWmpA%3D%3D)


# 3. Runtime Confinement Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/83585dfe2b312d739e2a5dc3af3bae33.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=Nb6%2FFCyiACJajLE4fJbIFyXB3jycXAVsKrWFh9EVpWJXFEEmk69JTUZ0q83emK8FMLGJy%2BUJhV15yQgLtb9kaA%2BISKzHs9vYeI%2BI6jYNMOfCQk0GnrtiC0fAn3MCxTWYOHEcJmHX3X0P4ZV6D%2Fcr%2Fgm91bSQj2Qr2FdpsCuX%2F%2B3jWFfRiZSCTDVsy3esEJR0TpnvfQjeaLdIK0KBA6CYs%2FftD6jSlMrGEsCdVkmOZVsRh56VPVD3oFChckW53%2BCh9R7NQmDaRFkXI2A9V4oEdrOM0HUoiQuPj08cfZH%2BATNCGUvZPqbIu1EUKc5y3n3Bg8KrhZXNSTK%2Frmwi%2BsdqtA%3D%3D)


ทุกๆ การรัน Container ใน Environment จำเป็นต้องระบุว่า Container นั้นจะใช้ Resource ของ Platform หรือ Infrastructure เท่าไร?

	- เช่น Python  1 Container จะใช้ cpu, memory, networking เท่าไร? ซึ่งสามารถกำหนดได้

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/c4a6bc84689dc129827dae8d15656937.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=pEsXEGY8NCrj7tWJGcClHxB%2FERvM3CBzpHHDwqN0HYoaJbIFtcME1mFGdi42kIFoVUYFvvXpoqvhrOsfIxTa%2FcFHGTzdjPtj69nrVHOZDxjCH9uctXbQpSTHGVH%2FLEsq5iaL0k3SqVHT3CMjGnBzrxtBi0NssJ%2B2Gztov0XbLOQi1G88ZN1zpsGYsD%2F2P7AOSSj1AAEop0rwAqcjq72M9ne%2FSpRXs57F6aZDVXbW5nYy6qLF%2FaqJ9Kk4flvbkxQheGrFGlZ7UMme9ksKnWhDCQBH7IXn9bxCho8mw6P%2FKJgsCbCFLUPlbwZ1K%2BU1si1NDybmBJtRkkjbYCMsQ9XQ%2Bw%3D%3D)

	- เพื่อทำให้ Platform รู้ว่าการจะ running Container เพิ่มขึ้นจะต้องใช้ Resource ประมาณไหน แล้ว Platform จะไปตามหา Node ที่เพียงพอจะ running Container
	- เช่นการทำ Auto Scaling ต้องทราบว่าแต่ละ Container จะใช้ resouce (cpu, memory, ..) อีกเท่าไร เพื่อที่จะให้ Platform provide และจัดการให้
	- K8s จะรู้ว่า Node มี Resource Usage เท่าไรทำให้รู้ว่าการเพิ่ม Pod ใหม่ควรเพิ่มไปที่ Node ไหน หรือ การเพิ่ม Container เข้าไปใน Pod ควรเพิ่มที่ Pod ไหน

	# 4. High Observability Principle


	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f7b36a9db98a593f8293c58e820b9155.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=djrdq3yNhGiVLn0oa8FJfSeQvvjPw4kOIv%2ByX3muyZRG8gQn2%2F5fthIj6x%2FABxYfzE56WyUzQL%2B7jSb1DGNPEL69Z86Hsi%2FAHAeTHczrQPHRC16%2B2k%2Bv5bqrz9Y2qEMvMe8YWCUvWNijBUHQen7YF15qZdx9togfWg%2F4MRTvroDw68%2BAgfKOMQM74JFRi3p2TqEyQLeDnyUjlcrp2quIFc95XpicgKs7DDTlALkr6OrPsMasAbXqQ5hzlOz2YsBKy%2BJcTxBwyom52gOiCmXviaRQ01acxImLws7pEvLI0Bsi8G0anPilqbC0VdqwVlgiz77%2F602PP0mfNjkruA6a1w%3D%3D)


Container จะต้องเปิดเผยช่องทางให้ตรวจสอบว่า Container นั้นกำลังทำงานอยู่หรือไม่

- Process health ตรวจสอบส่วนของ Health Check
	- readiness: ตรวจสอบว่า Container พร้อมที่จะรับ Traffic หรือยัง
	- liveness: ตรวจสอบว่า Container ยัง Running อยู่รึป่าวถ้าไม่ Platform จะสั่ง Start ใหม่
- metrics
	- tracing: Container ที่เราพัฒนาควรจะมีช่องทางเพื่อให้เราเก็บเส้นทางคำขอต่างๆ เช่น ทำ Tracing ดูว่า Request จาก Client มี Workload ที่ Container ไหนบ้าง มากที่ที่สุด
	- logs: บันทึกเหตุการณ์ต่างๆ
- k8s ใช้ readiness, liveness ตรวจสอบว่า Container กำลังทำงานอยู่หรือพร้อมรับ Traffic แล้วหรือยัง
	- Liveness Probe: K8s ใช้ตรวจสอบว่า Container กำลังทำงานอยู่หรือไม่ หาก Liveness Probe ล้มเหลว K8s จะสั่ง Restart Container นั้น
	- Readiness Probe: K8s ใช้ตรวจสอบว่า Container พร้อมรับ Traffic หรือยัง หาก Readiness Probe ล้มเหลว K8s จะ หยุดส่ง Traffic เข้า Pod นั้น

		```yaml
		apiVersion: v1
		kind: Pod
		metadata:
		  name: observability-pod
		spec:
		  containers:
		  - name: my-app
		    image: my-app:latest
		    ports:
		    - containerPort: 8080
		    livenessProbe: # ตรวจสอบว่า Container ยังทำงานอยู่
		      httpGet:
		        path: /healthz 
		        port: 8080
		      initialDelaySeconds: 5
		      periodSeconds: 5
		    readinessProbe: # ตรวจสอบว่า Container พร้อมรับ Traffic หรือยัง
		      httpGet:
		        path: /ready
		        port: 8080
		      initialDelaySeconds: 10
		      periodSeconds: 5
		```


		[bookmark](https://medium.com/sirisoft/kubernetes-zero-2-hero-ep-5-%E0%B8%AA%E0%B8%A3%E0%B9%89%E0%B8%B2%E0%B8%87-health-check-%E0%B8%94%E0%B9%89%E0%B8%A7%E0%B8%A2-liveness-readiness-%E0%B9%81%E0%B8%A5%E0%B8%B0-%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B9%83%E0%B8%8A%E0%B9%89%E0%B8%87%E0%B8%B2%E0%B8%99-rollout-79975f616f72)


# 5. Life Cycle Conformance Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/3e4b585ca403eda6e786109379f3e374.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=qVfw93skTfIaS9PrjnYwqX8%2Fal1Z84gkO7lufWAmqVF9spZL%2Bk51ytVhmeL51UDpUf%2B0BJC%2FqRog5btq7%2BPB9tU1SKNNzPla%2FhINMRYgwy0wpuvfcZSRYsqt2AuCE73r6NH%2Fb5ERD7hSrN%2FOZkkr2isgKpq5Ltm0qweONJobi9S4boAKRkVk4odfmpGFZS%2FcDAYZK4leyEPaS%2FQoDTzd1Gq3a9XSgAh2e%2BQZHE2oO9e8RjYEqPb0mnpx9QvlpzJ2gvYg81pWRkkTKkx%2Fusvt6vbqo0EL1%2BDnpTFSgcgXc1mm0LJbFnFlZzgXX%2F%2BbWRCcTsJcJNNvj2qZgLKm2LgTBQ%3D%3D)


LCP คือการควบคุมเหตุการณ์ที่ส่งมาจาก platform (Host) เช่น K8s เข้าสู่ container เพื่อจัดการ Application Lifecycle, Container ต้องเปิดเผยช่องทาง APIs เพื่อให้ Platform สามารถ อ่านข้อมูล จาก Container ได้ เช่น

- การอัพเกรด version ของ Container เพื่อเพิ่มฟีเจอร์ใน Application จะต้องสั่งให้ Application หยุดก่อนจึงค่อยอัพเกรด

ซึ่งจะมีส่วนประกอบดังนี้

- SIGTERM เป็นสัญญาณที่บอก Container ว่าถึงเวลาต้องปิดตัวแล้ว แต่ให้เวลา Container ในการ Cleanup เช่น DB Close Connection, Process Request ที่ค้างอยู่
- SIGKILL เป็นสัญญาณที่สั่ง Force Kill Process ทันทีโดยไม่แจ้งเตือนล่วงหน้า, Container จะหยุดทันที
- preStop คือ คำสั่งที่จะถูกเรียกใช้ก่อนที่สัญญาณ SIGTERM จะถูกส่งไปยัง Container, สามารถที่จะใส่คำสั่งเพื่อจัดการอะไรบางอย่างก่อนการ Stop Container ได้ (ก่อนสัญญาณ SIGTERM จะถูกส่งไป Container)
- postSrart คือ คำสั่งที่จะถูกเรียกใช้หลัง Container ถูกสร้างขึ้น เช่นการตั้ง Instruction ว่าอยากให้ Container ทำงานอะไร เมื่อกระบวนการนี้เสร็จ Container จะส่งสัญญาณ readiness เพื่อบอกว่าพร้อมรับ traffic แล้ว

# 6. Image Immutability Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/473181fa59e21833e59e680315efaf68.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=GiKKs6g%2Fx%2B6r7bhioByNK9uU9ak1%2FjwDOhXPiGR7j0OfQ5MO33lHYr%2B7W1njOE2%2BF3VrSRqZwptt5Xr30%2BsOFSJ3kQWwxQxCp%2FYyU%2F3GKoxbMB3IPEK9jPlK1higHk%2BRLo7Gve7i43CM06%2FQPiGemP3RYFgYU%2BZCNdtiPLSSij7NJ%2F1rkYhRxS60CLgyKXIbANlaqBx31dGqh24SG58Sf9fF4WAg5BEfc6iNB2crQYiEzpCL7QGi5kVLANQTpRGst3yquya%2BCFEgmkSaJtnChk0Sor8GKDCSxP5QWIPKAlO0EO1JYT7uLugQ%2BNQC7a%2Fgs9ldRhmpdOAPOP5Iob2uYw%3D%3D)


(Image สร้างครั้งเดียวใช้ได้ทุก Environments) ทุก Container ที่ทำงานแต่ละ Environment, Container เหล่านั้นจะถูก build จาก Image แค่ครั้งเดียว โดยทำการ Configuration ภายนอก และไม่อยู่ใน Container เช่น

- ใน Dev Environment อยากใช้ DataBase สำหรับ Dev ENV ทำ External Configure โดยใช้ .env file แล้วทำการ parsing เข้าไปด้วยในขั้นตอนที่สั่งรัน Container โดยสามารถกำหนด .env file สำหรับแต่ละ Environment ได้เช่น
	- .env.dev
	- .env.test
	- .env.prod
- ใน K8s ก็จะมีการใช้ object ConfigMap, Secret ในการกำหนดค่าต่างๆ แล้วส่งให้ Container ใช้งาน

# 7. Process Disposability Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b308d6440068e17e954587c02d0d13c6.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=NcGnt8xqeTMIYN59zl%2BCCvmXQe5a0auQImOmvYVp%2BwQoElewtgDOWNBZhvm4BGzCFzL4HlKJI5oa4zDoEEeWwygxNG82OsUXYvB%2BVbhPqXUk5W1J2Tofd8PLtYgOv1IbuRSPuuFsDbk29OemRmrBtQaKfM9Od70QaqbkoYD4mfdJdpEsWQaaj8lxOYnJ99IUuB8hMRGtdXIgeGKRJ5beTrf6UOsS9ylsTEp67lgATTX8PdwS2lfSH3WkhLabrLS8frr6Yv9Wy5X3oj%2BIpQM%2FFlYcMoOCwH4REFdKsStLctXTHABS%2BXa1zMS5O4I9Q3E31D6%2FRobHW3Jpv%2FdviQdkaQ%3D%3D)


Container ควรถูกออกแบบมาให้สามารถ ถูกเริ่มต้น หรือถูกหยุด ได้อย่างรวดเร็ว และสามารถถูกทำลายทิ้งไป  Disposed ได้ตลอดเวลาโดยไม่ส่งผลกระทบต่อระบบ

- Container ที่รันใน Environment ของเราจะต้องพร้อมถูก Replace ใหม่เสมอ เช่น
	- การ Deploy New Application Version เมื่อ K8s ดึง Image ด้วย Tag ใหม่ ตัวของ Container จะถูกสร้างขึ้นใหม่ เมื่อพร้อมทำงานแล้วจะสั่งให้ Container ตัวเก่าหยุดทำงาน
