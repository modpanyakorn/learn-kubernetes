
# Deployment Pipeline


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/46a3206eedccdd116def3c367b1da5d3.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=FGLCQ6jUVvreyslzKHNpfdhD0BcNDr%2FVcPjKgffmz%2FpRDbbNTEbABEtJ04RdiTW3p%2B8B%2B0IQlM8OScRlu60DyvQYwUv5wmfwIFF%2BbQ4uRRtSDViQBD1Iu%2FbyborZ144oG2sxw0WKPGfA4EeaY%2Bi2JxJhZrErP4Ke%2Bpy4vFF2u4n%2B%2Fw8zKHLfgx1%2B8wN%2Bvq%2BU1T8ZfiqcOH96JZ8nusT%2BLGLYOtPXVaoIc8soYNpOM83p58erhPO52chSzTxhzDh8kc%2BGsl6KqSs82TYdQLofUQb%2FqmTIYOi5AGvHs3lf9vJdMdJGlr%2FpTM0%2FamTELi%2FjFEWw5Ur7SiIyLYpZEK%2FuTA%3D%3D)


# Deployment Pipeline with K8s


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/e11d18ff9091cbbb2ade65ce8e00b96e.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=J3LWKBGI0UqRUsByVkhXTM8VCsqnCiQ%2Bx%2F9OL0ZASWhD2IxLeeGNMq794HinrE2il3%2ByBLKd56Jy68IRA2BuY8b9FiEAjgVJu8lTDwWIt%2FkKipp63huPTvZudaryo3zfV1AJaijcr35fK%2F8JajVEgEYPQDACc%2BWqaa2loUOSc4DI0p%2FvvYOV2nx1yVdTU6d7B7BjWy4boNlFrJ9fbAuebtyCcQ%2BGTzcXwV1S54oH1xiZSix3baOQVblCZV0cSj2UyNSIiief6JQdXM36mCxYs1qbXpK%2BLKkLonzFrln4Poj4ntp5jnDliGqucn%2BFYtNX1s9NJ%2BUX5jgO2i7CyIF3oQ%3D%3D)

- ของที่อยู่บน Production ไม่ควรจะมาพังทีหลัง
- K8s จะมาช่วยในส่วนการทำให้ Stable, k8s มีความสามารถในการ Deploy อย่างรวดเร็ว ถ้าเราใช้ Recreate strategy
- Pod ใหม่ทั้งหมดจะแทนที่ Pod เก่าแล้วลบออก เมื่อ Pod ใหม่สามารถที่จะ Start ได้เสร็จสมบูรณ์

# Package Management for K8s


เวลาที่เรา Deploy App on K8s เราต้องย้ายจากกระบวนการการทำงานเป็น step ให้เป็น configuration (IaC) เช่นบอกว่ามี Component อะไรบ้าง, Deployment, Service, App แล้วนำมาประกอบกันให้เป็น Environment


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/e3c367899ac1bf17d8c10e62ce2fcf31.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=pBdHexJwYx3t6Q%2Ft2WvL50dHDxG5alSdejTTB2WPCX9YXj03Ndq%2Fdxqn8jEuzhZUE9TKHI8DH7EV%2B7HG6S41aw%2FFFRD%2B3PP6uiwMgQGD5QWhtjqaI1P67jo9naR%2F175RXYRr63SP1tkCGucCrNlgJ0A0xCDl3jPyF4rFVbzpyBl3pYyB0dQ0RORWXbOQCloorT1rskejaqSBL5U35wLsNmvoYxIo9s6Bk2OmxwuYMIl5YNLDzWphLCWjXpr%2B%2FDqB4IkDP%2BZ6dCJSbJIXw9ZhAwX0lgNDE4ps4meVKfSnLkOTGh6iZVEUTn7LvXCQcWzI3B9eHrmrd5wvjvFsQYrEHg%3D%3D)

- การติดตั้ง Software Package บน K8s ทำไม่ง่าย เช่น Prometheus, ELK จะมี Configure ที่แตกต่างกันไป
- ต้อง Design ออกแบบ Software Package ให้เหมาะกับ Env ของเรา เช่น DataBaseเราจะ Configure ยังไงให้เหามะกับ Env ของเราเช่น ถ้าเจอ High Load จะทำยังไง, การเก็บ logs สำหรับ Stateful App จะทำยังไง
- ซึ่งจะทำให้กระบวนการเหล่านี้ทำได้ยากขึ้น และมีแนวคิดที่ว่าเราสามารถที่จะรวมกระบวนการเหล่านี้เป็น file ที่สามารถนำไปใช้ได้ง่ายไหม? สามารถที่จะทำเป็นกลุ่มใหญ่ๆ กลุ่มเดียวได้ไหม แล้ว push file นั้นขึ้น repository เพื่อให้คนอื่นๆ สามารถใช้งานได้ง่ายขึ้น
	- Software ที่เจอปัญหาเยอะๆ แบบนี้คือ: Monitoring tools, DataBase เช่น Elastic Search, MongoDB, MySQl การที่เราจะสร้าง Cluster ของ MySQL ต้องดูว่า yaml file ที่เราต้องเขียนต้องเป็นยังไง
	- ซึ่งเราควรทำเป็น Configure เก็บไว้ใช้ได้ (ชุดของ Configuration)

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/9953a96ac6a6302064b15cd8bacdb46c.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=TeuYzHE%2F%2FlDFTc2X4CeCGTTawmVwN3nRSKo867Dg%2BVAJFmbM7S9wqf5wNeOx5QXVHxd2a7KSRqzwbAqZYzJo6A%2Fiyq8f0SZpxfAReolSsBy9TWZgLKsHMvwHfZV4lEi3pKRZthyJzL%2FFuqIxq8mE5sVAYCDsqNVL7f1FA%2BUhsUj2stj1guxzH8JU%2Fx74firHowfigB5DqZrAGrVMXAQca11R9dM5i%2BUwCSwFzt0gWfn%2FEr1YNMUNEeBzSh7FzXCwQKPKlJzwtq6kyRkPdj6f2MJs1jXqHCJIH26eyYG0GDS4rmFlFD4XxykhdmLA5i8%2FMX8ZVZ1R58fFP2pTppA7IA%3D%3D)


# Helm Chart (Template Engine)


Helm Chart เป็น Template หรือที่รวมเอาไฟล์ Configuration ทั้งหมดของ Kubernetes (เช่น Deployment, Service, ConfigMap, Ingress) ที่จำเป็นต่อการติดตั้งแอปพลิเคชันหนึ่งๆ เข้าไว้ด้วยกัน


# Template Engine


การที่เราทำ Microservice มี Application หลายๆ Services ทำงานร่วมกัน กระจาย Load แนวคิดของ Microservice ทำงานแค่ 1 อย่าง จึงทำให้มี Service เยอะมากๆ 


การที่เราจะใช้ K8s มา serve Microservice จะต้องเขียน Configure file 


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/d6968cce81164f66bfd1143e6e803824.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=P0kZDm6mWvBotlAi5mxd1ggjsRSvyEFVXDek3zbZs9W%2FKmi8ondzuY1t6HT5LpiIZ%2BbumLonXvnGaED7MWTyyDRa1%2F%2BASwk4EBo8JTB8sz50xmQ0Jy%2BuR41Idwdq%2FvXdniTjzkVC%2BnMHP0Mqg%2FD3BNS%2FYscG3OYMvbaHKI6NnXcVSvTS6bcoRCbyorWB3RhTklkBghfWNCwRZ5VZmQbfTLClvyLWJ1e7ITKcu7mcypDKTL2VGN5KIVA7gfRZmTX6zO47AZ8XEyJTQglrKE2rGdGowAw0CQIPgtNnkmuggOmIU0QUIPnpZiXW4Ih%2Fu%2BAgGQwEKz0fOJRDhVvWOMH5YA%3D%3D)

- ถ้าสังเกตดีๆ จะเห็นว่ามเราจะต้องกำหนด Key:valueไว้เยอะมากๆ และซ้ำกัน เช่น Deployment กับ template สามารถที่จะ connect กันได้เลยโดยไม่ต้องระบุ Labels Selector

# YAML (Deployment and Service) file almost the same


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a4f5632359d9d891eb661096cd3a47c4.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=f6YKOZhAv4sjMBRbg7apTIaEWFk4vj8%2FtN4%2F%2FhwE%2BYQwb3SlqbCydXhWzXZd0NvRg0%2Fj4mNomuTJQLx6Ehzk7Zk0vdF9fb6Pd%2FUC9R3ZE8udAjXgLjTdS%2FJCp5gJoh0drtLXJ5i57M2Kc4cRZfaBp8Q4BlG2z81Id2%2FodrZgX9h%2BeB6iYSVyBauSqHsn9I7k0NklJjduFzyOdhz%2FormuRBvsfdz7v7n1PB5ZhX32POUnz%2F85L0cJCV7aR5Xw1S9Q6K%2FcGr%2BV3x44ChwPW%2F9ZJFX9zTe2T2H9OTcUxUjSIV2eOTwT%2F1A8v%2Fbt7U2tsLvxhfK4iuNqp8l6%2F38%2F24oO2A%3D%3D)

- ถ้าเราจะมี Application ที่เป็น Monitoring มา Monitor Service พวกนี้ทั้งหมด มันจะต้อง Configure เยอะมากๆ และต้องมี file แบบนี้ซ้ำๆ กันไปเรื่อยๆ

## ลองดูความต่างของไฟล์


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b288070b9082c68f1141c5c6661985e4.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=DUXCqLEUyfL6fzxytIbpJy883yGXH6HXaCauMR%2B96MlGtb7n29wq5RvZ%2B8S%2FA3%2FL%2FJ8tnjZxK0d8EU%2Fpd2QMha4%2BHCgTmwdQrnCALE0wdveTpGKsIxQGYbvRaM20eftLQa6ODyPq9%2Bbh6Sw8xTGQgbnsAsEQmVdgBtHyaK6ug1%2BuH%2FknQrifHtEfFvelysjjkdIf7wyYR5uYI2JkqctzJjaQmXKDd4mH%2BBjiKfE83hd1eG6kWKG0tvxTT56CD6kJJsNdUiNmf9jr6MZavZL1eF5Op73dYTZwLIuWzBnCJDm%2FRV1KeKWT7sW%2FPzU9qr1YLSkwu53MZH08ZyCXEeSZaA%3D%3D)

- เราเปลี่ยน Static Script ให้เป็น Dynamic Script แทนที่จะต้องเขียนไฟล์ใหม่ๆ ซ้ำๆ กันและแก้แค่ Value บางอย่างเข้าไป

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/27338200845a1e1fc56079e5f9d83130.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=TemG0Mn4vXFNJ0g7FoQ128PURVKqQT6gn2vDOgh7dlnzGVAv4goLREdmT0HFEF11Crd9aq483CgRUtCUZL1Y%2FEMEP95sTJ4VF5dopOVfKSiFee8AmejAb7bnVfV9jUB7XjeIqKq2ytxqluQSZK8zxkX2VIDPYLvXObrfjlrccRvVJPAXksJYeBAA3bX2Y3JLL55TvVI3vhKyxVfeqVhytRxl274pgpAvTJbDItLt26yxa0LSp7NzW4zczlPJuy1LqkQPWwsH0kmCMvsFRzTbdAi9LdN7ZJ8MT9vZj4P7GfwJCaR5oCydHzMrz0t%2BcQc997JsDhL0H4qGJq0%2Bo4E8mw%3D%3D)

- การเปลี่ยนเป็น Template นี้ทำให้ Chart เมื่อคนอื่นนำ Chart นี้ไปใช้ สามารถเปลี่ยนชื่อ, Image, หรือ port ได้ง่าย ๆ แค่แก้ไขค่าในไฟล์ yam หรือ Command Line โดยไม่ต้องแก้ไขไฟล์ Template หลัก (Template Engine)

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f9c9d27a115103cbb47181b3cc3f4494.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=WkvWFqfuNUKjXnQuGyACU7XwTb1sU2bN%2FWUxZpqF0GCKvfLWV%2F3tpiXiAgeHLw5VoLXVsodiBXFkdJJrcTBuBuEIJ%2Bz3g2EqGbbc2FX12dxMTZ6G8hTH0uQ8tfI5LTlAC6fJgF1VIlZu0dcLF3YUGtz97vxH%2FQJb6NdtDhiiQydPRmaAOTEAHHZ55%2FN0EONZYJhKd2iv66YKRTmbG3BojwCzxUux38WUNlcvAEfAQ6Fj6gIZN1unLgtUjE7D%2FcxqQf%2FDh6OQxmKC7NXu%2Bz0JGEErY3oa1zPFUDpi%2B2EqknV2G%2BYJh14JulyrHxhi4LETZjIU9jBbdEHwzYhCVraCng%3D%3D)

- เราไม่ได้มีแค่ Template แบบๆ เดียว เราสามารถทำ Template ของ Environment ได้
- สิ่งที่แตกต่างกันในแต่ละ Environment เช่น
	- Replica
	- Image
	- Limit CPU, Memory, …
- ที่เหลือก็จะเป็นสิ่งที่เหมือนกัน ทำให้การนำกลับมาใช้ใหม่มีข้อดีกว่าการเขียนแบบ Static Script เยอะกว่ามาก

# Release Management


วิธีการในการ Release Software ผ่าน Helm จะใช้ `helm install <chartname>`  ซึ่งหากมี Dependencies ที่ Configure จะใช้ Helm ก็จะจัดการให้

- ทุกๆ ครั้งที่มีการทำ Deployment จะเกิดการเก็บ History

	![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f7ed810394acf0dcc65b75da4a7a0e67.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=hVj7VHDBYlN8EUxsNoUBTm7WCpEo7FU0nm2MM71zjYn2j%2B52HAm3a6DG7e4rW0ZyqWgobp6eN1V7lmL8pB2bEmZ74F4zJK2ylZcsrwm0DkcGhB7XzzZrYg3CixS0KmPD1paX85%2FF9n3fGi0ooQ5gpE5WWT4snqj5%2FyfJV81GHVD%2BsVLeekSny2DSTIUNM8DJ4c8XzQAML0HaRGA1pPqCGq1PF6WFKnZlY%2BuMiGSee5yNFDNiff9HXy%2F0Ca3WC7lldqHStXtHPnpH3Nh94g4d98M61E0NyO02dRwWJ2AlciKJY1WLtc8CkpkYSj8zcMv2S2yUztyOd5FwOfpAEnxK%2Bw%3D%3D)

- ถ้าเกิดปัญหาทีหลังสามารถที่จะสั่ง Rollback เพื่อนำ Version เก่ากลับมาได้
