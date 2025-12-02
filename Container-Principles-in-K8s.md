
# 1. Single Concern Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/486e29f55dd5997b076494de63363268.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=G14KIjDDFKUR%2F6CcBUdjDEdDHsglXA65olg%2FkfRryCiJ696mnBLGMIv7IG3N3gUrhKyyI3bW72rJLJBtysSEyzfnwQ6jehya3MdB3d1mLCFDAOOWQsJNYYzrlx8QKqE6rqClbCmsY0SgyVTvrS%2FTNFFGyBV6x7jgeXxtQuHRUFH68Ow5A4mkAFUfbHmZd3nSDQKayUtbcUGKYTr%2Fo67qLKNajAZpOLOM0I1xYuoS00neTT2ZCSresoRF2WY1vRXjojzHrRGDA4jLOwBWYtkkuCO8zgAPT6%2Fehj8JisgHJrhhBZMPXHXIvVZXfgHiobvqc%2BxPH8gxWHVaGJjiIYPt5A%3D%3D)


Single Concern Principle หลักการความรับผิดชอบเดียว

- แต่ละ Container ควรมีความรับผิดชอบเดียว Single Concern และทำสิ่งนั้นให้ดีที่สุด
- Concern ในที่นี้หมายถึงชุดฟังก์ชันการทำงานเฉพาะอย่างของแอปพลิเคชัน
- หลักการนี้แนะนำให้ รันฟังก์ชันแต่ละอย่างของแอปพลิเคชันใน Container ที่แยกจากกัน
	- ง่ายต่อการ Manage, แก้ไข, Scaling
- สัมพันธ์กับ K8s
- Best Practice ใน Kubernetes คือการใช้ 1 Pod ต่อ 1 Service เพื่อให้ Pod นั้นทำหน้าที่ตามหลัก Single Concern Principle

# 2. Self-Containment Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/2aae796a4af596f14349adaa634b77c9.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=KFhltm9A16H8WC0MBe8D%2BJw2QGE64LqIyrIRqBkxfZBRyrd89txTJbs5V1KzX7F1AAi1M7r%2B5xg%2FkZfPiHAF1SZGAzz36ohp%2BiQSuPrS8JTyQKwHd0G20V6roQ%2BVfotN8wBZJd8FFkk2bd1Aewswk4e%2FBmfzLxjscG3hSTq8ymUpd9sevd9QVBKxy14T7lSsErTwN2WWNFDogOQo%2BS1kxlFZoCKClM0SQSP197RFvegFYkEbit%2FWPgBGnpbAOaSwWXLLG6sYSo6mcybexPCdBVePfvhTfh1YkAap%2B%2BgXQKJp7eA8mjhozKLfTD7%2FVDTGwss1arYPAVhQ5FnCOXPaqg%3D%3D)


Container image ควรมีทุก dependency ที่จำเป็นตอน build-time ยกเว้น config, secret อื่นๆ ในช่วงที่ใช้ใน Run Time เพื่อให้ container ไม่พึ่ง environment เฉพาะเจาะจง เช่น ไม่ควรพึ่งพา Libraries หรือ Dependencies ที่ติดตั้งอยู่บน Host Machine (Worker Node)

- Container Image ที่สร้างขึ้นแล้ว ไม่ควรเปลี่ยนแปลงระหว่าง Environment เพื่อให้การ Deploy ทุกที่สามารถทำซ้ำได้
- Configuration ไม่ควรถูก Build ทั้งหมดเข้าไปตั้งแตช่วง Build Time เพราะจะทำให้การแยก Environment ทำได้ยากขึ้น
- ใน K8s ก็จะมี object ที่นำมาใช้ในการเก็บ Configuration ต่างๆ
	- ConfigMap, Secret เพื่อส่งค่าเข้าไปใน Container ช่วง Run time

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/e37c5cccb13a23b84b5778b7a7c98571.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=Exa1rWjplrljXgDZcT1HFu2%2BrZwbnXCAFU1ph24UN%2FoMabggENjNAILxKXDCyx3UJ7i5g6Hjt%2FNaLTOeDUZGKneGNAydUI4zxLAbxWW4EkXSWilLdLuTmXFtg2LKeM%2FVpb%2Bhm%2BvSbeYqhafNl7ea%2Fh9CdHtACecvWIho7TwvRt051JE2ww7Fb5pCD5t85N2TFUpIbxMTofHKzuSQNJqEBrvm1sBG9wQLZNj8I3VYxO2ytDrsqyzKFXA7pTiyptRFuJJ8u9c9YdB1hPn3LUZ8S41mr0nQwtiC5x8AuV5%2BV0AYDtasgyyNSYnKm91TMM6uPLIK4BEMATU23mHDCzPBaQ%3D%3D)

	- Storage ใช้ Volumes เช่น PersistentVolumeClaim ในการเก็บ data ภายนอก Container โดยที่ถ้า Container ถูกลบออก, down, crash ก็ยังคงอยู่

		![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/7343b4b19b96487166f7b042af863a76.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=TZ2cXwAvmXTgpVbweDiaElpopLHEObSiK%2ByvvKJpjEZxpq6Q4fygkKPn%2FbwLbGRBMO0dfjkgGg2vSWLLMh%2FBvxbZUkUJ4Xshh9%2BiwanojlR9Yf9g206hZcFpfpADoDyNv%2B6%2BYwGAzlc1pw4nLaSHulHHePv75H%2Fn8WzhhhDuRjftzMFcd5zTjW2KlqNVUhw5UAf1AKTqXr0LKNt2gVSxMVrH9SFMjhl%2B4CLv6kxPr%2F0ByNiz0H%2F9YP9UfxIiC%2F0KMMLrjYk7tqjZlkFkEqY3BE%2FhDblWSp2PfhOdlZtq3CetHUlHoUI%2FPtNZkQLMXRoYD5I8BZnHJD6yr8HEMoVPJg%3D%3D)


# 3. Runtime Confinement Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/617866225b178163cc593f379d6e5e26.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=q3k7Btf547wOxkOgrYcuCl68t86AJMqMDwc9FA%2FXljrsjNOsmDEYvzdY3LdmbNwdxxoCWOI57u%2FvAjdLfpLIhp2RqrVpbMRXCgKHEOLQqq3JsYzVEwloYw97DAkaHQ21xgB27J8w8ILDf%2F8IBOBetZtbjjkT7Wb%2F41jn%2Bvk4jPBSh2QARzKKo3lMDCNO4lGGWIHAK9cimH4F888GkP3rHrhR2Q1IDksYVPT07qrXbH62zYPMS4UiynHh6LU1bYfGwUvg0zD4VWny2CM9Q6N7DDGG5z%2FzScmwdnuMpVm6cxiAE9XbESjB2l7NJNOf8jnsKv%2FuqJGq%2FgS%2FKV1AhfvwqA%3D%3D)


ทุกๆ การรัน Container ใน Environment จำเป็นต้องระบุว่า Container นั้นจะใช้ Resource ของ Platform หรือ Infrastructure เท่าไร?

- เช่น Python  1 Container จะใช้ cpu, memory, networking เท่าไร? ซึ่งสามารถกำหนดได้

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/92d68cfad78792c639d9beb0c3715321.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=jAuqiMFfr21HKoIWfAHoP3M6zDZWJpQZYFRtos2gBhXvp9uLYBbbD7gFawij78vqZSdfe95sVPxUk9Pszd4cXotz%2BLT12tAfhS66CrnaS3flebCODAe7z1cvEMDUWtImpEmCIqHBB46XOWkE%2Bm6qGCXVF1b3MrrdnF3P1qwe88pMPiR1n0b1aUcoRnNgaKgHcgdesuVSGzmZDs41n9SVHulbG9WBBzCgAeDmrYmOVdwkn8%2Bv3sKjw3e%2BXYyOxQXRmkKoS1IurAXEQT%2ByFIqAKJJ7zDu%2FX0zXZ%2BY80teEG2Bm6sw3iZp%2F2nyfsvaXinxnn3vX%2FUZM4W59Wo4EOtJS8w%3D%3D)

- เพื่อทำให้ Platform รู้ว่าการจะ running Container เพิ่มขึ้นจะต้องใช้ Resource ประมาณไหน แล้ว Platform จะไปตามหา Node ที่เพียงพอจะ running Container
- เช่นการทำ Auto Scaling ต้องทราบว่าแต่ละ Container จะใช้ resouce (cpu, memory, ..) อีกเท่าไร เพื่อที่จะให้ Platform provide และจัดการให้
- K8s จะรู้ว่า Node มี Resource Usage เท่าไรทำให้รู้ว่าการเพิ่ม Pod ใหม่ควรเพิ่มไปที่ Node ไหน หรือ การเพิ่ม Container เข้าไปใน Pod ควรเพิ่มที่ Pod ไหน

# 4. High Observability Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/eb36ab786124d33c07a5af5f325d1ab0.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=h4gcIPDl6j33aiEDNsML82e%2BMZYfajwgR%2FFnhxnbAr3jMHSnbXv6iAKXxJy8DRdQ3PsEH98ZHFtUdcE13X%2BjqPEealEXDt%2F7Ro5ZSkRAOCP%2FUKyJH%2FUOLQAQfirXVYVWnjTYkDjlxLBbcrstzZscf1gcxSdGm3h3mcRmx3aXDIANxA7Dx32wvqqxSWmimFxU%2FbqDiQ%2BJsKYMdLksm22g30rNuKs0CeOAJXaOIl%2FA%2FdiQQZdRF2b60WUwX%2FW00%2BNeqfrSCoMmmOGgpGGd7Nuc8u5dfMsuyduppuV4vQMi33YaF5T5j3ZV6ZbzJs3PBF%2FOrxFW9k%2BQw8p4VtQFZlS1YA%3D%3D)


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


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/9c0d833f18665d748f11bbc08e548005.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=VbtJoBbXncav30wzSC9iDj8qYnUuRv6a7pcdl3CRBdsu49I7pPW35Slf%2BWAeT%2Fu9Tc8yThDzmXGnJLWZT4a8Yb8XdG7jUSgbWMkenmgwar8vvs3xrwCRkvDxGMH5oYTHrBu%2BQ7wiZXUh%2F5KVdv3aq6qZDjamL5a%2FhDB72IX2K6t7rn4mATA9ke9%2Fz8VZbAOFxkVCVkxJPHLRgrl1%2BHSM4Gc9F9ZhQxcKMxuSsIWE57w7KbRxER3zw5F1kxIdWOhqb2oUw5Ke77JGX7q9cfL%2FXvpoeH6BJLYBbhhCYKP099QTHL7FfSntvmwn528Lkv%2F3qpyTqP4MLJgYYm8iv%2BQ9Kw%3D%3D)


LCP คือการควบคุมเหตุการณ์ที่ส่งมาจาก platform (Host) เช่น K8s เข้าสู่ container เพื่อจัดการ Application Lifecycle, Container ต้องเปิดเผยช่องทาง APIs เพื่อให้ Platform สามารถ อ่านข้อมูล จาก Container ได้ เช่น

- การอัพเกรด version ของ Container เพื่อเพิ่มฟีเจอร์ใน Application จะต้องสั่งให้ Application หยุดก่อนจึงค่อยอัพเกรด

ซึ่งจะมีส่วนประกอบดังนี้

- SIGTERM เป็นสัญญาณที่บอก Container ว่าถึงเวลาต้องปิดตัวแล้ว แต่ให้เวลา Container ในการ Cleanup เช่น DB Close Connection, Process Request ที่ค้างอยู่
- SIGKILL เป็นสัญญาณที่สั่ง Force Kill Process ทันทีโดยไม่แจ้งเตือนล่วงหน้า, Container จะหยุดทันที
- preStop คือ คำสั่งที่จะถูกเรียกใช้ก่อนที่สัญญาณ SIGTERM จะถูกส่งไปยัง Container, สามารถที่จะใส่คำสั่งเพื่อจัดการอะไรบางอย่างก่อนการ Stop Container ได้ (ก่อนสัญญาณ SIGTERM จะถูกส่งไป Container)
- postSrart คือ คำสั่งที่จะถูกเรียกใช้หลัง Container ถูกสร้างขึ้น เช่นการตั้ง Instruction ว่าอยากให้ Container ทำงานอะไร เมื่อกระบวนการนี้เสร็จ Container จะส่งสัญญาณ readiness เพื่อบอกว่าพร้อมรับ traffic แล้ว

# 6. Image Immutability Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/30ef0f3b18d5616cdc737ed777f2bdfc.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=nGpWBGZ9U0rMpj9NVoxmD9eSiQFVyvI6m%2BwY0R8FfxF%2FodA0vBLvt%2FipAZC8wpOBKtoDniTb4eAIpCcX%2BJVLo2pbqWHEo7JioJwyFBTNvyaX6XGFKNfVekDyPaK45c8hBBAdpiB9b46iqRgnQeA%2FdV9hCwHZDXErl06xf4I7xBMIv1Wwg7JS%2BoswtBfe2ZtTFCW29ecJNKFCKKEcwHpHpZMmAEFMLA7FqufNokVkR0LseJi9dB2svFxsKkZHc98TMAL4Ld8shDmYsMlbxHABYJi7kUd6sT1%2F1ThXr%2FtcK%2BzqcYujr7Q2e%2BvqybcAr4PkkXpBJVlqujmHDPyi1RE3EQ%3D%3D)


(Image สร้างครั้งเดียวใช้ได้ทุก Environments) ทุก Container ที่ทำงานแต่ละ Environment, Container เหล่านั้นจะถูก build จาก Image แค่ครั้งเดียว โดยทำการ Configuration ภายนอก และไม่อยู่ใน Container เช่น

- ใน Dev Environment อยากใช้ DataBase สำหรับ Dev ENV ทำ External Configure โดยใช้ .env file แล้วทำการ parsing เข้าไปด้วยในขั้นตอนที่สั่งรัน Container โดยสามารถกำหนด .env file สำหรับแต่ละ Environment ได้เช่น
	- .env.dev
	- .env.test
	- .env.prod
- ใน K8s ก็จะมีการใช้ object ConfigMap, Secret ในการกำหนดค่าต่างๆ แล้วส่งให้ Container ใช้งาน

# 7. Process Disposability Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/031d32f9699606e87d754accdc435933.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=Ym7sfkztm31ggIbUtVfHZRdQ465ZW8kheLZewnAFMXhBDRjr3%2FB%2BUv08%2BD5ZMXzkchCwX9QYX9aIdofw2apofcqabX4IlY1DVW6V0W0PUgz8jSlXytP8Wk37UvrCZJFlCEnHOzcRC1uJjW7S9XwT9aWZEpEfRejHhOHX8VUSzQy658dKot82VKs8zdlicrQh2Bbx72Za1g7Ck0TQDIKvzuSToVS7vIKhTJudBGxzKkOjciFHrUNYvOq84L2rGwAJJ%2Fh%2FxlCfITz3ZhFNFCOMQo7MRxfu7vUhRykfjcT3d9s3GvCxmvznXbvVFRfcDNOoqPTCK9vxziCBJhxWNujP%2Bw%3D%3D)


Container ควรถูกออกแบบมาให้สามารถ ถูกเริ่มต้น หรือถูกหยุด ได้อย่างรวดเร็ว และสามารถถูกทำลายทิ้งไป  Disposed ได้ตลอดเวลาโดยไม่ส่งผลกระทบต่อระบบ

- Container ที่รันใน Environment ของเราจะต้องพร้อมถูก Replace ใหม่เสมอ เช่น
	- การ Deploy New Application Version เมื่อ K8s ดึง Image ด้วย Tag ใหม่ ตัวของ Container จะถูกสร้างขึ้นใหม่ เมื่อพร้อมทำงานแล้วจะสั่งให้ Container ตัวเก่าหยุดทำงาน
