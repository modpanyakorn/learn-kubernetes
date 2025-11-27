
# Self-Container Principle


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a43fc940736d89abd998af3713659175.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=qciFdyIubB%2BpYjpp4No0PJk0k6VO6H3O3fWpDmBdtZcSSVs%2FbgDPL7J9%2FdvMbn%2BFzCTJxrTDBgSs153rmDqiaS6YX66j3sewY6iqzlEoy%2FUKy%2B%2F7Kk%2FQ8wHSlf4HGfwnDVd54Spw5Gny51R3skDwD4jjyEt5G%2BpndsvE2PcDDCO1D1%2BEywvAyaTupEqcaGY3Ol%2BEKA4HfIKO6iJQSJMAMItP%2BU0BaGxm6GCw67wEc%2BiU1tbSwPfC6BoKNjhrKe09Hlu2wSl6%2B3dToKZQcZOqdXqMyX60ZtD39cIh2DoPLDyux1DEaZqlQ8Qu9eanp2DZkZDmFhBfJJQPd7J3Z1t1Aw%3D%3D)


Example

- สมมุติว่า Application ของเราเชื่อมต่อไป Database แล้วมี Service ที่ depend Database service เยอะมากๆ หาก Database เปลี่ยนไปเช่น Provider, Storage type คำถามคือของที่ depend Database service นั้นอยู่จะต้องปรับเปลี่ยน configuration ตามไปหรือไม่ หรือ จะต้องปรับเปลี่ยนตัวเองเพื่อให้ Connect ได้
- ถ้ามีการเปลี่ยนชื่อ service จาก db-service → mongo-db-service ตัวของ my-app-service ก็จะไม่สามารถใช้ หรือ เชื่อมต่อไปยัง Db service ได้
- วิธีแก้คือ: เราไปเปลี่ยนแปลง configure ของ my-app-service แล้ว build เป็น Container image ใหม่ ปรับเปลี่ยน version ให้เชื่อมต่อกับ DB Service, ซึ่ง Application ของเรายึดติดกับ DB Service มากเกินไปทำให้ต้อง Hard Code แล้ว build container image เป็น ver ใหม่ถึงจะแก้ Configuration ได้

# ConfigMap API object

- K8s มี ConfigMap คือ Object API ที่ใช้จัดเก็บข้อมูลที่ไม่เป็นความลับแบบ Key:value โดยที่ Pod สามารถใช้ ConfigMap เป็น Environment Variable, cmd arguments หรือ configure files, ConfigMap ช่วยให้แยก Configuration ออกจาก Container Environment ได้

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/22a37d8733eeec74af7171fce19012f6.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=WRL36fTSp5wvKyhXjHUQAIKLFX0N3vVF%2F3UAOYODmYS0tUtVKVudaVxlMCyma3ChRWggSvhZnIiVtkQrLaOzfGgvWWSPZ0TR31CNqQPSgQDTjhKPz35abImLH%2F2CHqqEEZ4CcAUijwcClC%2B5B5HPtovH6q6CXZYhASA5QeHPyXSquba2xS5ZisfHQSIItcLnpipTe5DsnSGqg7k0s%2BGP5NQKQWq6vcyRnH%2Fa%2FgaxfUHK4gamo6TyUEfT1m6LhPsD7yQOelmGNfXSEYlsGBuoi0xCi2g2U3kyieqCTgeE0PayYeovzs9AcD4OTeLJoCn%2BxBHmLI8h6I7IUOHLNn0quQ%3D%3D)

- แทนที่เราจะใส่ Configure เข้าไปใน Application เราแยก Configure เก็บใน ConfigMap แทน เราก็เปลี่ยน DB_URL เป็น Service ใหม่ที่ต้องการเปลี่ยนได้เลย
- คำแนะนำ! ห้ามนำ Sensitive Data (username, password, credentials) เราสามารถที่จะอ่านข้อมูลของ ConfigMap ได้ แล้วเราจะเก็บไว้ที่ไหน?

# SECRET API object


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/826c97f0f383b5ea6ade51d15a966657.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=gsNJ9fVO9JsQ6Ed3BCye0NDwHiSdcx2HevdkkPH68XTkPH4Nytb2jn1AE9poM%2Fke4S6NVK7Zjt%2FGpsq%2F5uWVOK7A3etfWC6yQSf7g3ULDE%2BdU3c3kRHPEhPzqJFpj7DZR52DEF%2FeOorfB5%2BvwLNqNnx0bTCwwDDsu7FRcZL1AacQNwhU9nwG5UPYOxHEKFiDWNsUutlNv%2BQstuZAmpD3Y7zGDJZEuqYa0jv9nhjK6Qno9fpa2H8xh6mi%2B6S1rYCrqXIHK9lXVzOePvNxRQOptgaTAoAJyKZ3mqYajg7x3t9uY1WC5ZktLe0doJwEUjQRUr%2BofjwJhGhIqIMgR2HPXQ%3D%3D)

- Secret คือ object ที่รวบรวม sensitive data เช่น password, token, key เราไม่จำเป็นต้อง inject หรือ hard code ใส่ไว้ใน Application Source Code ของตัวเองเลย
- เมื่อเรา applied secret เข้าไปกับ Application ของเราผ่านทาง SECRET object จะถูก encode/decode ตอนเก็บ/เรียกใช้

![450b2c57-ff72-45b0-8040-be1b26e02b4b.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/8ff18833f542dab9b03f3308d855f199.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=cC%2BjSBmWNYjxCcL1kVlTaJWFP8Ae8Wb0J5bp1TbbP9JKCWSVM4mqCEWILGz%2Bs2hugyHpeHiu7YKjWWUMAluNxdbCvYBi6gffdpXd1cwmu4CbTd6ytM96DLK%2FE2eRAXlJfrEJCRX%2FQyG0yW%2FOkYW0dDZdS91FMabL%2Fxr2xXaRwT%2Bv7WQQcDgTtKnw%2Bvnbx%2FPQ0cR8XcocBLA7xGz4BPp9fyBQaMpQHssOVN3pGC6W3gtjlHeiRN2etDSINALShge2vPnpjdQi36VTxYHqmlkGAz0yBse%2BQVX3iwWyCwovpiiwFikNO3napHyLRY0XlJZf1IZz85KBmfV209DefPnuTw%3D%3D)


# Volume


K8s support volume หลายประเภทมากๆ Local Storage, Network File Storage, Cloud Storage

- File บน Disk ของ Container เป็นไฟล์ชั่วคราว ซึ่งทำให้มีปัญหาสำหรับ Application เมื่อ container ทำงาน เช่นปัญหา
	- สูญเสียไฟล์เมื่อ Container Crashed
	- ปัญหาที่เกิดจากการแชร์ไฟล์ระหว่าง Container ที่ทำงานร่วมกันใน Pod
- การแยก Volume ช่วยแก้หัญหาของทั้งสองได้
- k8s รองรับ volume หลายประเภท Pod สามารถใช้ Volume ได้กี่ประเภทพร้อมๆ กันก็ได้
- Volume Ephemeral จะมีอายุการใช้งานเท่ากับ Pod เมื่อ Pod หยุดทำงาน k8s จะทำลาย Volume
- Volume Persistent จะมีอายุการใช้งานเกินกว่า Pod เมื่อ Pod หยุดทำงาน k8s จะไม่ทำลาย Volume
	- สำหรับ Volume ใดๆ ก็ตามใน Pod ที่กำหนด ข้อมูลจะถูกเก็บไว้ตลอดการ Restart Container

## How Volume Working?


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/955615f2c99b0ec21b7e9ba0f38e36c7.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=Jv8Ly6uKW2bMhfi0yLofOmrf89BCxH5Tqa8sWFnvKHocFoUWi%2FkXv32tXSleoyMCY7HQ%2FXvYahSMFOhQErc23f%2FVTiAnHP0745XZqVNPMZQ3MbTdLluwgAO6oRqho40opt4NC2M%2FdhZrBFuWlp566W3n7oL2Ny2s0%2Bb9TY%2F3SkioVRuab9lbqsN84ZyOFYh4eN69%2FVmAE0FJGSgbuks3a4Ofw4jf0mCL8fsiSTQQWspFp35zgRTfqY6pVsKRlmlUMDDdJ8A0urmwqXNkNTNiz68F9GRW%2BW4ZwTXAn%2FOrryxiUc0QURmJeRiVLywO3OhEfU4EId5Ne6Oh86vmK%2B2fFg%3D%3D)

1. ถ้าเกิดเราไม่ได้ mount volume ออกมาใช้แค่ Storage ใน Container แต่ถ้า Pod died ลงไป data ก็จะหายไป
2. ถ้าเราจะทำ Persistent Data ต้องทำ Volume แยกออกมาเฉพาะในแต่ละ Pod
3. ถ้ามี External Storage เราสามารถนำมา Mapping กับ Volume ได้ให้เป็น Persistent ถึงแม้ว่า Pod จะ died ไปแต่ Volume ก็ยังจะคงอยู๋

# K8s Workloads Deployments and Statefulset


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/7ff9938799520f4addc96f3fe9e81402.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=hnHQNYiAa2escRMiKK%2FbNJu%2BGw2eTvEiKhT2STyNHONlSJix9R1SNMdHqFdnEWJ%2FY8HAI7I71FMONo8tBS1da%2FdW%2BfkFph8092AGFhXrYNd5LMcDs92KbL9Ttv8lv%2BxyOyKSaazPLUk2ScvWKYOplNqXLl6ffpUX3rWrYFOmy5pW4VClxgpv5i5g%2FJvaibVFtMPil2HZS3Xhl%2BZmM06JOpQNgUoQ1vTFIIuP5Lc7AcCJ%2BZv%2FmhXnGmimp%2BOX4puAGC13kaeoeORnRMseuQutfkfVls03vpDGpnNWA0K2Ac%2B%2FvAqthtQDTR4jgZQVeMy2aOvK7tVqTKN6klqBbYJx%2BQ%3D%3D)

- เราบอกว่าการ Deploy Application เราสามารถ Deploy หน่วยที่เล็กที่สุดคือ Pod ถ้าในกรณีที่ Pod มีปัญหา K8s จะสร้าง Pod ขึ้นมาใหม่, แต่ว่าการ Design Application ที่ดีทุกๆ จุดควรจะมี Redundancy node ควรจะมี replication ซึ่ง Pod ก็ควรมีการทำ Redundancy เหมือนกัน
- ซึ่งเมื่อมี Pod ใหม่สร้างขึ้นมาแล้วถุก Register เข้าไปใน Service ที่วางไว้ และ Service จะรู้จัก Pod ใหม่ที่สร้างขึ้นมา
- หน้าที่ของ Service →มี Fix IP Address ไม่ว่าจะย้ายไปที่ไหนก็จะใช้ Fix IP Address เมื่อมี Traffic เข้ามาจะกระจายโหลดไปยัง Pod
	1. ไม่ได้มีหน้าที่ Manage Pod
	2. ไม่ได้ดูว่า Pod อยู่ครบ
	3. ถ้า Pod ใด Pod นึง died, Service ก็จะไม่ forward traffic ไปหา
- ใน K8s มีการสร้าง object อีกตัวชื่อ Deployment ใช้ในการบริหารจัดการ และดู Blueprint ของ Pod ว่าเป็นยังไงบ้าง

# Deployments object

- Deployment จะมีการอัพเดต Pod และ ReplicaSet
- เราเตรียมการตัดสินใจหรือ policy ใน Deployment Controller, deployment controller จะควบคุมและเปลี่ยนแปลง state ให้เป็น state ที่ต้องการและควบคุมได้ เราสามารถที่จะกำหนด ReplicaSets ใหม่ หรือ ลบ Deployments เดิมออกและใช้ Resource เดิมกับการ Deploy ใหม่ได้
- เช่นกำหนดไว้ว่า Pod ต้องมี 2 Pod ตัวของ Deployment Controller ต้องพยายามทำให้ Podคงที่มีจำนวน 2 Pod ตามที่ตั้งไว้

### Scenario


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/ac58d0156ea6226a6eb343e7331fbad6.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=OzcInOtFa80uDNCsHobogxnWnI%2F3mo1Ix2Gu58mYw7V1QDtf3DXG4VLuJrnVsbaRVMIXFbDuvSk2ykGRDM4BT2ijXqZ2YNqQ6zzbq1pXWlJwmHp1yzHWEz2%2F2k80DI8WQ0DOCU1ZWXLzR9p%2BApF0sIY93ffEs068RqGeT61bqKkPn%2Frm5K47DyHua6de3vDQ7zCQgS6H4VI60zegdb5Co0vWmwGqlCTg0cgjS3RJ7PEkDsmbILM932%2B%2F6DgCY0U%2F0j%2Bbv8XE0%2B99ceU9OdWf3Wfzl%2BBGHOBLF3%2By0iu%2B1HflNEuOWH4QBbjwDi7Ib1fUIqCy6%2BbLEgRpGqPsJOLEeA%3D%3D)

1. ตัวอย่าง: เรามี Pod 2 ตัวและ Deployment Control จะต้องควบคุม Replica หรือ Application ให้มีอย่างน้อย 2

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/5bc5cc43964769654a2d39461d292ed4.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=opjzwLCesUMYohBKmMQpjZGYACfzho7mrK8nyAFGpUjAtvf6at0e9aAyOocVFYlpvxH5eu16CavHI8Aj6kaIIkKLuu2dn0uahVp5t7cT9Ii094ES45fTMAFqAY1l8QWm0GJteLjWYk5sH1XjwoOYfcp8tPIY6Kq0ZCB11IiIKMT30tgKkIvUTBbCVl83aQajrPdkUyf8nYP%2BQmmI3Pu9Bv9sWbvpsW4k9HxWjHSeCpPGxIjgRDrNwiogx3rKLwr1X2yivGTw%2BZDYkt8wmc1uqWsXQYR1kaNzw%2Fo1qzOD%2FzMNN6BNdBK03wYuh7mUIREHdE9nJCIhi6XUeJ83j0cWSg%3D%3D)

2. และเมื่อ Pod ใด Pod นึงในสองตัวนี้เกิด died หรือ crached ตัวของ Deployment จะทำการสร้าง Podใหม่ขึ้นมา

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b8d8c5e45bc8bc095510ea7a8df5fa84.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=ORuxHJ%2FfKy7oXwJ945x5v7u8Ac11nAu8k0dcCNCAhfsBZL4QjywYHezpnTRPcMYtpmb1REMQbQYHzPcQQ9yDtY1nI7JKuVUr2pXmD8oeScozYvWK7hrHx6rc0apA47jOMpWbEyAzpOVz0IeMuYz3OfD3KOAbUiziD3Kbe6akgyY6EChZ45g9zdgllSO1XDl3smUCx%2Bg13nETxEtyzZq37S5mAQ18CHP%2BfGdyLULbcmGaRYJ5JwhW1RFu8dmZCBS4ZCpuiogv64rlUvK%2FewxLQFU6tGWleLRbdeZ6wWeEGZvYRZl%2FemyUigCr3hTpajOyuZKZLSaTxs18nR0Ss6V%2FTw%3D%3D)

1. เมื่อมี Pod ใหม่เกิดขึ้นมาแล้วมันจะทำการ Forward Traffic ไป Pod นั้น โดยถึงแม้ว่าจะไม่ได้ Start Pod ใหม่ที่ Physical Machine เดิม แต่ว่า Deployment ยอมรับให้สร้าง Pod ใหม่ที่มี 2 ReplicaSet ที่ Physical Machine อีกตัวได้
	1. Application ที่พร้อมจะ died หรือ down แล้วไม่มี logs ออกมา (ในกรณีที่เราไม่ได้กำหนดหรือสร้างให้ Application Expose logs) เราจะเรียก Application แบบนี้ว่า Stateless Application
	2. Stateful Application คือ Application ที่บอกว่าจะมี Action ต่อไปจะเป็นอะไรหรือมี logs อะไรบางอย่างออกมาว่า status ตอนนั้นของ application เป็นยังไง เช่น
		1. Database application ไม่สามารถที่จะใช้ Replica ผ่าน Deployment ได้ คือ Deployment จะไปสร้างไว้อีกทีนึง และตัวของ Storage ที่เก็บข้อมูลจะหายไป

k8s มี object หนึ่งที่สามารถจัดการ Stateful Application ได้คือ STATEFULSETS


# STATEFULSETS object


STATEFULSETS คือ workload API object ใช้จัดการ stateful applications จัดการ deployment และ scale set ของ Pod ได้ และรับประกันความเฉพาะตัวของกลุ่ม Pods เหล่านั้น, Stateful จะรักษา Identify ให้กับ Pod แต่ละ Pod, Pod เหล่านี้สร้าวขึ้นจาก Spec เดียวกัน แต่ไม่สามารถใช้แทนกันได้, Pod แต่ละ Pod จะมีการระบุ Spec แบบถาวรที่รักษาเก็บไว้ตลอดทุกการ Scheduling ใหม่ทุกครั้ง

- จัดการการ Deploy, State Scaling
- การันตรีว่า Pod นั้นอยู่ที่ไหน, มีกี่ Pod, มี Connection แบบไหน จะต้องทำได้แบบเดิม

## Stateful-set


Database ที่รองรับ Application แบบ stateful set ได้ ตัวอย่าง MySQL, mongoDB, elasticsearch


การทำ stateful set คือการนำ replica, pod หลายๆ ตัวมาทำงานด้วยกัน


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/2038acf86f569cb6334cbe9f7717c4b0.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=Oj7Wvblio%2BUGWnByBaD%2Bv0ui83xwZr1d2RYd8y80XaDy04d1csvy2XfFYqbegQXBL9uzOoHortFNIzJirkwoaHhK%2FAa8Tro7roudgqMr7kXMXaYeDc5qj4Cb78MUHaZ8Rh%2BOmjiTcYr9Ai9aM0ZCCyiM8rVbuxtmpxhC15riVYJzld9FBOIOJ44c6k%2Brz6lGv46RTBaiKnlq%2FBGBuU51F5LQpA4MzPkEeYM%2BrjFTxvuwGw%2FlhsRn7jl361PnG36ZIzIoW8dV1X5rLnqjLeXHMDdDwv4V%2FCO%2FmetmHTZyus89rL5XPkUWiP1msJo6eBzVplGAs01KkpG0wy7a%2FW8dww%3D%3D)

- !แต่ว่า StatefulSets config and manage ยาก ซึ่งส่วนใหญ่จะใช้ Database ที่อยู่ด้านนอก Kubernetes Environment
- K8s จะใช้งานได้ดีกับ Stateless Application (Application ที่ died ได้เสมอ, ไม่เก็บ logs, สร้างลบ ได้ง่าย) แต่ว่า Stateful ก็สามารถทำได้ แต่ต้องเข้าใจการ configure เพื่อให้ใช้ k8s กับ stateful application ได้จริงๆ เป็นแค่ concept application deployment ไม่ได้เป็น requirement ที่ k8s ต้องการหรือบอก user ไว้

# High Available Architecture


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/4aade1f83438ef6fd567b375d0967961.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=TuwYnQd3h16TE9DT03WOhf%2BM0t07%2F%2Blh09NKiBghY0Ct8rHK26M4vSLYC%2BljugY2jnLE0OKj%2FoD4NsRdbrRriplx0jspAbkAg1AW5MtdwV%2BwUofzsit0kPm5iyakoxmWdhQZsRczinA7YxqidCbG%2Fqe%2BJwgHztu%2BK47J%2BqCjRjHJUtQB8i8aoNGhQy%2FyPJc%2BrUZBrpwsMqn%2BO6om4DRQE2JZWGKuhjaOgMImsodRjX9Zs%2FaGJEsQGRZFj4mqkBY4eKKHT3zjFbr6THFd49IxtslWhaMq9LcgWX5gUzGIxCf1SIVKz%2BPCL3fMGTrXq1J8ExMD9F6%2BpdnYQyqNOadusw%3D%3D)

1. User เข้าจาก Ingress, Ingress จะดูว่า request ลักษณะแบบนี้ url นี้ต้องไปที่ Service ไหน?
2. Service นั้นมีการทำ Load Balance เพราะมี replica 2 ตัว, Load Balance จะทำหน้าที่ forward traffic ไปหา Pod นั้นเอง
3. Application แต่ละตัวก่อนที่จะเชื่อมต่อกับ Service อื่นจะอ่านข้อมูลจาก ConfigMap หรือ SECRET
4. และการเชื่อมต่อกันภายในจะคุยกันผ่าน Service ไม่ผ่าน IP Address โดยตรง, Service จะเป็น object ที่บอกว่าถ้ามี request แบบนี้จะบอกว่าให้ communicate กับ Pod ไหน

![938de6d5-8133-48b7-83d3-5da3b8db4c95.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/3e240de73d70eb22cc0b32f7d41a9d6d.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=RA7T7GpnqDG1vYbW1AkKb245%2Ft7nfiKaYazyOQpvm5rEb3i1RJiYi2l6zeeEbj%2BN7%2FC3EIaSRiGOb9%2FxgYva00d1KPnB9Xp4rr6o44dXWXa2rAHtQXS%2FYHoTcCX8FmAfS5Mzp4%2F7k5VhsYZpyaFFbrQx6lhEtwKpagLLO6UVhR7WS3tTTTrwVFbRFPMvxj5zGWrTxiFelzv10jDp54ZbyCn7SYz4DsTE04DAArf54Md3PaIo2qCjCfO2ZogzOB6pe8crNVe5z7PZqaaqFoE1ugWG%2BEdXxCDFpeZ3zKIVek194XrYqTq%2BByL2D9k9ZyNkPI9fJaC22NfvFnL%2FxFmLqQ%3D%3D)


ถ้าเกิดว่า Physical Server เกิด died หรือ down k8s จะตามหา Physical Machine ที่สามารถ replicate หรือ สร้าง Pods ใหม่บน Physical machine นั้นได้

