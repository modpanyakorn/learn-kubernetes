
# What is Kubernetes


K8s พัฒนาจาก Google เนื่องจากมี Container Services มหาศาลใน Data Center ของ Google 


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/5cc1a7c23bd00adce97f7770597b8e76.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=PJvmEOiEkuvr3zu3xEkrHm4FBEVzE8yRWB4Vp7VkKN09fHn1ONr5pLIy9VoH308WcZYBqLqYH7C%2BRsCaoM50uRORI2OPAwlxCnbQ9Q80HIuII7J%2B0ETedV4Ktar7REuwsGAJfC7MKbNU3AxNqxafcg%2B7horX7KW%2B%2FXZG1GgQw03f1pHIXqla8sVnLyf355Nl1U3zdRwv4cW%2F2UGSVyKSBdICXMD5EU0aDvCM%2BQcFXDB0yoMeR%2B1KOJ4mBWVUh2yTGB3vhh4weyJruagumMn3LRK7WifZl2Dbm5rlpX1m0Qz1dKQSF68nYlP0ms2Ewf9EKBfU5RoGZZtSwmnYNFtVvg%3D%3D)


ง และได้สร้าง K8s ขึ้นมา สามารถที่จะจัดการ และขยายเพื่อให้รองรับ Workload ของ Container ซึ่งสามารถที่จะ Declarative และทำงานได้แบบ Automate

- K8s คือ k1, middle8, s1
- K8s จะตัดสินใจว่าจะรัน Container บนเครื่องไหนใน Cluster
- K8s กำหนด IP address และ DNS name ให้กับ container และสามารถกระจาย Traffic ไปยังหลายๆ Instances ของ application ได้

### Cloud Native 


Community ที่พัฒนาเครื่องมือขึ้นมาโดยไม่ยึดติดกับ Cloud Provider ในการทำงานของเครื่องมือตัวนั้น


# Deployment


## Traditional Deployment


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/24722b8b7520ed6e542c0c7f37480601.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=h9rWYXNMzH0NzhwWGnTjVp6VTUl8FF65tVNYDepVvFl4s1ahFnRaW4CTAKopJ5BOeCUpuWWo8bHkHnOqwiIdOn2yJpRFozd214fFetZTcqhhaZNiJopySuzYeVqA2yU26GJff1cG7Vhig%2BO4KKKO1F%2FcldjkykzPaRIaFZpKKwIFvrpL3USvoEjWU4adaAg36h8A9PWWELgF6REQo%2FMgjQXiLQ1jfvlqpFhjBVfJoMbklxGfOUR6tTPn2WlSlHMm6tlHBhFpN61bmswfw5oDVmVK8oDLW2S7tFxMbm9JQEDwNLv4GQHrEyaK98fA8K8kT49GKprFNCSFcpQY6tMqcg%3D%3D)

- แบบเดิมจะรัน applications บน physical server ไม่สามารถที่จะแบ่ง resource สำหรับ application ได้ใน physical server

## Virtualized Deployment


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/315a8f2a24adcdc75899eb7b44ce6f6c.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=IwKvkQUppKPOKs3RV4q4p9CfU0tp7OH%2Fcr1gTT9DnTFR0X4A2rkYgjh4YSI%2FGpsrvUfgvpSq6TvMkNCGWwQ5T26tp6eE11nq2yn6uzM0O4rWcoujuTpLFAJYB1U7AbCDS2P4BtjOlB6vSL9bSEqt9h8%2FgaL%2B1rmzRQD9alnFoluVGsWMvSwB8nZGCzwoCkDbeHvVBDFF1ZNYmWQeesY7oXTY19oDNfVv3DlwA4nxuQX0P93Dfe%2BaPHwY2cnIx4S8ikUtqOYxXa3nvyJMY42mJC8W2vZzRfz%2FUrs%2BiSzcIgDPujeU%2BUzwvvOJnr%2BfLAhec1NIx1ykba72C94zJVrZsA%3D%3D)

- แบ่ง Resource เป็น VM หลายๆ ตัวใน Machine เดียว ช่วยให้สามารถแบ่ง Environment, Scalable สามารถที่จะเพิ่มหรืออัปเดตแอพพลิเคชั่นได้ง่าน ลด Hardware Cost ได้
- ข้อเสียคือ overhead กิน Resource เยอะมากๆ

## Container Deployment


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/578329756ea7c4ac919617868e0e674e.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=ZI46NQ2pBPxDIf%2FEZ2KpoH4pbVw2jQmxzygiVv7dFdvXZJhriU2yjK6Y3tTGjItpxyRlXM8f72UEzeKWXiE15Az%2FG%2FkPh2XpdHMvijNt5m919xO1ux1ACYaceXwONqeRY8u7nIPF3nHZizdFkwhPmxOWoqOJVtkJlGsc%2BR7q4E%2F%2FYlL4AtZVTnNgSZlugUJ%2FaVVDr4Be%2Bc5VdJSZYfD0lhKaOjAejQTYlZnRGYIaRJvZ1lEIlLGLxKjurSa9ZZYw04GkiC%2Bm%2B2VYfY3g7qZO08YrWQU5ZwfGEekeCbcutO3clXc%2FRnGW%2Fg35P8hbkYN4%2FP%2FstMdH1CHi16oIcYAxhw%3D%3D)

- เหมือนกับ VM แต่สามารถที่จะ Share OS รหะหว่าง Application ได้ แทนที่จะแบ่ง OS ออกทุกๆ VM จึงทำให้ Container มีน้ำหนักที่เบา

## Microservices


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/5d6a3812b8459fdbc0b4a3a3f59bfc81.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=pOccKcBgnSFmJEI5IHKWMovNOVO5dfQDH7cH8MsYIEkeIMNldjXg%2F%2FbrGW6FQKa3cPCvBBGCr9pbBQK0YzrJJzQYRRpWfXCOoNpzS096KUoRmH%2BSRnZd4A5xCrzD45IOcsYg2Ps3EkRlSLcIZdtCi7A8B31krbQ5%2BS%2BVsWr3aE4YNtbVg0r24z8VSekpx0QeBPTz%2BxI%2Bg9MMdvAZKbMxeto6bTvPjf%2F8zNy2XmQ%2F5%2FXYfxSZze9M%2BQV3kFJh9LuxQyaYx3YqEfcRAm4BbA6KgO0ohSYdzfrlsF9yqGJQ0nuM4C8Fn%2FCIBybyVhl0I10cQ81QTpKuhnrjOH5C9g8M1Q%3D%3D)

- Monolith แบบเดิมคือ Scale ได้ยาก Service ทุกอย่างอยู่ใน Resource เดียวกัน และจัดการยาก
- โดย Containerized ทำให้ Application ของเราสามารถที่จะแยก Services ได้ และ Scale บาง Service ได้โดยที่เราไม่จำเป็นต้อง Scale Service ทั้งหมดที่ไม่จำเป็นต้อง Scale ทุก Services

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/bcb61bedee5a3664bf686d42ff8d77da.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=BlbfDGl20FVem6GsFChjVyGdmowuUsReW%2FqROJEH76aL2nas6I4i6RBX8mlW%2B6TOUBoSGNERJ%2BdXZjb4blRYcQi%2F85epm2X8ix4wfPGMTVwaNfC4pFKXlOjA4333IlzxMOU5xf6JWNNPyFltPcElh5iBRX%2F9et8H7Ft5Iu6UKzpiEp9I3XBTdsJUe0Lr8QP2nRhzHeHd7M4h%2Fxz%2F7ZKrMVVBBDled0nAYeOULoUxV6RMp8EdxNSwtKxLZTGGdLjFOv1Akc7EbB2lzFbvAb09dufjPjuH2rAggbrZOjOsJ0%2BEVlrAN7ahmTumv52qFyQy34TqekL41KvvK60hR9RzpQ%3D%3D)

- หากมี Service ไหน died ก็สามารถที่จะ Start ขึ้นมาใหม่ได้โดยไม่ส่งผลให้ Services อื่นๆ พังไปด้วย

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/6ec4fcd5c2015cbd4403cfb1d97068ca.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=KfI02K9h6Hf5oXc194rZH5VOi4JQ132a5VNa15QB2IM1ZRLE1%2B13bbUZs84JW7J4W%2FU3Ks2ZvVVbsl8pQjAV48O%2FJGXkL3uQbsvgTJkIfiDA6RXMFim6dzAKYN5YQshXtTQZOIekVpLL%2FtGvBEvn6XoiPxrrKGiG2Qmdzp%2FZSZbKVqXul2yb2%2ByPdbcoYcRekjsB8%2Fgd1w3hHOBtcZv7xJgs1QK2iuZAO994y0%2BW8A96fz3asPtB4DTecRAvjcVB9Ok08wAKQF4vRnjPHfXsYHQwX0RPscCeTtfvXCMBBHp0HSNNqsm7M%2F4ZEToUQtupVC5Hqmz68FltIxdcF1IJIg%3D%3D)

- สามารถที่จะ Scale Service เพิ่มหลายๆ Instances ได้สามารถที่จะ Scale up, Scale down ได้ ตาม Workload ของ Service นั้นๆ เลย

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/ff2811e4ef8ee9abc6682c8bcf3a1734.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=KJs7w%2BCNQ5f4uVO1if4n92z66245M6kUHobGFajOgsQ0jz%2BDryemt9V%2Fo7CYgg6%2FhKexbEVE6qWyyL8OWsX%2Bo5t1VCgj5haNLK5y7pg30j1Q9%2BVtHInu9Qov6hI4rSvCaEs0duP4xV62TkRNU1b2EDkvUTy0alYjh6VvL7xU9%2Bw94JFxhVeo5BoMKCfppY4lKSrqpCSSDCSFulKIsUP6%2BBEp3uQb3TEVjnC6jD%2Fzk6NzDSZSMUOFhxaDa2k6%2Fghhh17RQEmTMrwM897TUwWPlZuq1CuRJvX2RAMhggxi2RPADZXchOsgxtLCYzGBUNn4zbTywv0bGuQw5q2iQFz34w%3D%3D)

- หากเรามี Data ที่เก็บข้อมูลของ Service อยู่แล้วต้องการย้าย Infrastructure เช่น นำ Service ไป deploy อีก physical server หรือ cloud server สามารถที่จะย้ายไปได้ เพราะ Services ที่รันอยู่เป็น Code, Configuration เลยสามารถย้ายไปทำงานที่ Server อื่นๆ ได้ง่าย

# ทำไมถึงต้องใช้ K8s


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/b138662287f5fd92357e6ba400358264.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=iEGRCxSgtxUAUOhqOuFf78Pzs0H0nFOy%2Ftu4NWVFgTJKoiQhKA8Ks6XKI5ODUmkDBsX9zU%2BNRq0ZEqkrw1qr6T5ziQHxnJrMG3JFxiNb%2BbKq5gOVh9doeEIWsurDaM0LMDIfELDI2RNeV9gU1%2BEku465Pq5C5%2FKn%2FijvfAnuHdi%2FB0Uffd53qeHud3zxWUXUCaR%2FGodReoJFdwN6QNnuHyIqKljneOBs5VT2%2FYpoykiSB0Q4YDlOY7RX62APslw3XW4I6ET%2B8rSp%2F%2FvW9t8RWgydrfakk1qIfKLGTCOSN4TAk9a6sZ8C%2B9gb5NVlMBoVxpN%2BiM6TRzcRYMxpwq5ZGQ%3D%3D)

- การที่เพิ่ม Worker เพื่อรับจำนวน Workload ที่มากขึ้น ทุกๆ worker ที่สร้างขึ้นมาจะถูก push เข้าไปใน environment โดยที่เราไม่ต้องทำอะไรเลย
- Pod เพิ่มขึ้น K8s จะกระจาย Load ที่เข้ามาไปที่ Pod ใหม่ที่พึ่งถูสร้างขึ้นมาแบบ Automate เลย

## Storage Orchestration


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/8da4af632b7c9089ade46392018df504.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=QG8z5ccXSqsrQwb1Y7zEG3dM6VbZkZfc7OIkiBZUnA8Rze262m8ACaPxQk1zuS0B5JQgcrhUAJHkwIbCfoWPmc%2FMgyZ9DMVaasSxIp%2BmXJigsRmPSFB%2F4muB0yqdYoDmoOqjLVUSdo3fPmN%2BSwHu76wG1%2FtJxe%2B0Kt8za50rY%2BzXHW0sm1ljy3JVxrBQPOr8IUwDzKWmD5aVycH0Q2HULCsbn3UOfNYdw1DBuQxaRcmaidW77QQnqCDIu5xHOqdpg6%2BrAi6Ioq0NRiXjmrzijb1kWe93mc5xvh9rE%2Fhewv3Y%2FRq5fCsuTfLqbB3TZjF7yk2RjGmGuzsjsCjDLQIaVg%3D%3D)

- Kubernetes ช่วยให้สามารถติดตั้งระบบจัดเก็บข้อมูล ที่เราเลือกได้อัติโนมัติเช่น local storage, public cloud
- โดยที่ Pod ทั้งหมดสามารถใช้ Storage เดียวกันได้

## Automated rollouts and rollbacks


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a9ff98a6a4946efe74e6bdc2366a980d.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=mceLvSoimf1bjy500xNtw33k%2BVLIhH4nZyGAR%2F8nzi9ZGS6a%2FMy1e0YrH4YS9E%2B%2FdSPPKYekqsp%2B9pNzrcL4qa5ySp5bv2HU6FVG8J2l0%2FyKQwc56x%2BofwDnejR%2BOOgKCbGEnHLlC%2Bq2BIYH7Y9kxkKPSzzyLUpZaJPkgRuG6TafgxjO%2BPJH9yS%2BUiBeXB9xBLBb8Fqz%2BiMAADKrGe5iiQxVzJkhyLe9qz5vSsEcbu2SzIXKUe2dKNAcabuy%2FXKxeIIOcKOd9qnkTVoa6Jqh7OWvmPeJc9gC6PcdyQRu%2B2L%2FBoCJf8%2Bx0SOB6CnqhphiKVuNUNvPgxSWIOjH%2BEx2DQ%3D%3D)

- การปรับปรุง version ของ software เราจะค่อยๆ เพิ่มส่วนของ Version 1.1 เข้าไปหากทำงานได้ ก็จะค่อยๆ down version1.0 ลงไป และทำการ verify จากนั้นให้ k8s deploy version1.1 ได้เลย
- downtime น้อย
- หรืออีกวิธีคือ start ver1.1 ทั้งหมดให้ครบก่อนเมื่อเสร็จแล้วก็เปลี่ยนแปลง traffic, route ไปที่ ver1.1 แล้ว down ver1.0 ลงไป

## Automatic bin packing


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a105a024788b06488470266467bab9de.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=BzUmmPGD4wSn1sfmJbf5DLdy3L4leC49iGgZ3v8mlf7ojaLQq1ln%2BKepFwJpMUuGiJ784Wi%2FaLIV0OjDRmhNMPGP6KvZuy4rFEOsXEPZ%2B6wNvxIm7mcodH7dCQBE3A2WcD6ud3uRtaom0U%2B56pzgUC%2FaAvUCwHC0PBHK9FGLGh1YYUehz%2Fod%2FaLRTnyXI%2F8jC%2Bu7EUyMdUiv%2BlAKZXy4fky1LQ8IwOUo8MJ8mFAgHfpygfaIf86HClg2dgm7f4A3p2xKCGhDTye7GWC%2F6zinaEWPp0JV9fcWsE2KQwNBDRBtu3i0TnExjx7pZO0Gk92HI88epoKdhctlRTsUkh3VSw%3D%3D)

- จัดเตรียม Cluster Node ให้กับ k8s ซึ่งสามารถรันแบบ Containerized ได้ว่าแต่ละ Container ต้องการ CPU, Ram เท่าไร, k8s จะสามารถทำงานและ deploy container เพื่อให้ใช้งาน resource ได้มีประสิทธิภาพมากที่สุด
- ทุกๆ การเพิ่ม Pod เข้ามาใหม่ k8s จะจัดสรร Pod ให้รองรับกับความต้องการ นอกจากดู Resource utilize แล้ว, k8s จะมี policy เช่นหากมี Pod ลักษณะพิเศษที่ต้องการ GPU จะให้ pod นั้นใช้งานบน GPU (จัดการที่อยู่ชของ Pod ด้วย Algorithm)

## Self-healing


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/edd1e74070fac49252f161423815ffe8.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=L198OnDsOezQBQKQic0agZVRllPpmpGUU7uxmjtiriITQeJNuE7HtvAc5fS7e78PYvsUmeP%2Bn9D8C995YP3vHl40AKPL2%2Fkr1gXsQakE0o8Vmx3yz8o%2BNRvoHGQH%2FmiOQPObxdbcAybBn6DKwFCq%2F8pMOYCd1wcK2lnCoIJ9s2apRyiPtEtaX9vlo70OLEWEYPy40hhOBwyUvna2%2F%2FLlziBa0mi3j74mwcOkARIfrdOvgy6tjt58wDH2gTgPxgQYOhZj%2F1jJB79d6v03lB9nB%2BcGWg3xkrUDD%2FdqIlXMErIRK8rQydXEFUqYLPZdYkJtPhyaHaDLoYkubMJCLwZttg%3D%3D)

- K8s จะ restart containers ที่ fail, replaces containers, kill containers ที่ไม่ตอบสนองใน health check ที่ user กำหนดไว้ และไม่มีอะไรตอบสนองไปยัง client จนพร้อมจะใช้งาน
- Service ใด Service นึงเกิดมีปัญหาขึ้นมาโดย k8s มี (Control Manager) เป็น service นึงใน k8s คอย monitor อยู่ เมื่อใดที่ตรวจสอบเจอว่า service มีปัญหา เช่น รัน container แล้ว failed, control manager จะสั่งให้ restart service นั้นขึ้นมาใหม่
- ถ้า Start ไม่ได้ Control Manager จะสั่งให้ Scheduler สร้าง  Pod ใหม่ขึ้นมาทดแทนตัวเก่า

## Secret and configuration management


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/287164a763f58c000b4f0cc067c4f624.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=mzGrJZgUF5bwZmO9odKhLmT4U5ahxwoShSwq5pTuaKb0Qik0W7u8LeG%2FCH3N%2Fg5z4sEOyIDH6qTpQ1kzdzgKSr6oTkWsrLrzi1R49qIm5ybxdyNVdWfXmYp0hHFi1EMPMpXYnfib5eurq9aJ4NouWZXx91vqv2FiAZpG4P76MNArHHBPrazUgS7XePpTNvT39DTQDaMew7HFpmIAa1z42x9xCZ6CCBM5FMjwyHtTKyH9eBhCKi1aUeqYV6hzeDluuIZ0DjSpmRYLgqQsIrtdD0za8vSSKAjhunfG%2FlimlWKe9vzFmD8T9FQ8G7Z4SPh2esi9EhksSR7ZmHtd%2FvsD8Q%3D%3D)

- K8s สามารถที่จะจัดเก็บ Sensitive data เช่น passwords, OAuth tokens, ssh keys เราสามารถ deploy และ update secret และ application configure โดยที่ไม่ต้องสร้าง container image ใหม่ และไม่เปิดเผย secrets ใน stack configuration
- ข้อมูล secret จะถูก encode ไว้

# K8s things

- K8s ไม่ใช่ PaaS แบบครบวงจรดั้งเดิม แต่ logs, integrations เป็นหน้าที่ของ user ที่ต้องทำเอง
- ไม่ได้จำกัดประเภทของ Application ที่จะนำมารัน, แต่ Application ต้องเป็น conatinerized
- ใช้ source code และไม่สร้าง application ใหม่หรือ inject อะไรบางอย่างเข้าไปที่ทำให้ Application เปลี่ยนแปลง
- ไม่ได้ให้บริการระดับ Application (ไม่มี application service ถ้าต้องการเราต้องติดตั้งเอง
- k8s ไม่ได้บันทึก การตรวจสอบ หรือ การแจ้งเตือน
- k8s ไม่ได้จัดการ configure ของแต่ละภาษา
- k8s ไม่ทำเรื่องเกี่ยวกับ maintenance, management, self-healing
- k8s ไม่ได้มองตัวเองเป็น orchestrator ที่ต้องทำตาม step เราต้องเขียน configure, script ตามที่เราต้องการ

---


# พี่โจโจ้ Jumpbox


# 1. POD


หน่วยที่เล็กที่สุดที่สามารถ Deploy ได้บน K8s


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/7d3a051bbb68ce0d30547511b737cc6d.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=YKPuEwwVOjrrEQDeic8hKwPO1WT7OOChSyjf4K2ZE%2BPoGiz6m1GKe7lQOCUHZ8gVRoSrdqQdDIO%2BcaFnWHa3Ps17TRXyV19qvZaSD60ITnO51H8HF%2FkbWdgv2m6czdk9j1%2Fk591hnpAAM7GdOc9Ddk%2BbsPh1SkpSc0LBvBx6GBnDnQq1eGxgN7O2%2FBcUrPi33L2UnaOKebLXxfHtANNrMf3Bi7ngl5Ml6n6Z3JCA7%2F24FKtQhacxxwHuepL7BozV7THCBEGgUL1%2Fuhkwk%2BQatuXTPzyOOeaGTGvgE8uf0GPdWxOcUTOFpMYKdS6uKBgEuoxLrOB5spKIvsHRld%2BKXA%3D%3D)

- Pod จะมี Container หลายๆ Instances อยู้ด้านใน

## Storage


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/0aaafefce68ae9c53fdb55a511661f5a.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=pBOpl3CkGMY6NgAf0kgdGOyL7Etgz0%2Fjim6vbZXaDPfm29GSrIf5yrKnltXsmxnVRAauLuThaTfxptm3%2Bhvvi88lVvbKCiHQiXRZzaUVmC1jT5fF2u597qfpFvZv%2FoY4%2F8Kl0I9MLvf9wl7TjBVSoxfMWNwwSSvMlEAm9GQMWQF9kjG7DtIm0UBIdVQ1NuCKAXE4g7eUNpTQl%2BjrgeKV%2Ff9UaQ2hKIxX8etOWK9KulEqEvD0g%2Fuo56NIGZKOi1rUjK2tWx9Xp0PqEPlBHWV9NI35jC2SHTFdqmA2mKinVC5KzG4CHeeECQbSjQF8Vyw%2BDkNnCLsQKhxozEr%2FeWINvA%3D%3D)

- Container แต่ละตัวใน Pod สามารถที่จะ Access ของ Pod นั้นๆ ได้

## Container ใน Port สามารถทำอะไรได้บ้าง


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/637fc39e5206f01d4c4f1a018197c379.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=XBbDczvhYNE6qvRCA2o%2BV4wYgVBqMKhgdG3%2F5CGGeI7MG1plHAKoeeAqnMZ7p1nuNuAlVVyG0nRrsjFhHWPJ%2ButcDzuZnGAN%2BVez72JGNeNOp3jkeyxMNwk3KDWvc3FGyqiiFa2EVNCrQmJJOlutXDHlwnfSTRgWghZzKRk7dR%2BOBpyXBTd0NHsDFjiR3GwHz5EkiTUoD1%2Fcav6G3ES5CEjynamfqSL%2BMOnfAroMUnEsV9ITdX9eazVYXO16ckSyJpmIy22epH0nhhkeexkjtrssWCLJaw880Niu6%2Ft6GXNp4cyRC5OPZ%2FXi9McU76WFr7sg41S86u1DoPQUUPo6yQ%3D%3D)

- Ports
- image
- resources เช่น limit, request (cpu, memory)
- liifecycle เช่น จะมีการ restart, init ยังไง
- environmemt เช่น .env, config.json, …
- others อื่นๆ

# Deployment


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/afbf622ec0b74ce9db6d488f9414fcba.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=V%2BeHotA6kU8UfmIMry4aSu04bOu418tO%2BWGqarQikynq6wMoIZ10YNP8SirWLLiy0YZQd6QJfruETN1eV5sJvJJl29ylxE27zfX25g7gk0IzL68%2FfqDlgFDnl42PVmZTVsCguo6l%2FCkQwMU7ZG%2F%2FucNmNaYPLSkT%2FrqCI0He4%2Bcfl8kmQiNR0pbdHtdWL8ae6ryf4A4%2BtrBiNkFEgF04pQUIQvHGSl8B232wfZRQ%2FCjOt%2Ft9XXqreIeXYIMhtfGzcIaR8Nk5bphe1pMtx8aqgvEEwoOtHGd5T%2BCAoy7qoBO2LFO1gfIaeySkk6BSYlhFzF1C6cyYbTI4kV3wydAofA%3D%3D)

- Replica Set จะคอยดูว่าถ้า Pod died จะ Start ขึ้นมาใหม่ตามจำนวน Pod ที่กำหนดไว้

## Strategy 


เราจะมีกลยุทธิ์ยังไงให้ Deploy แล้ว down time —> zero down time


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/f7cb1a7c0c88fd828cf778aa281e3775.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=gnwI6%2FKJKAtHma5YMkWdJH0S2DpyVEGP2LacmwBcl4VxGvCBiy206aKQdRYKY7MgDjZRVjTQIqRBKAehyeJYFJCK1zETJe03RyZsjcrF0OFWVhGiv2JuYFiCL%2FFqpexD%2FPe%2BWXckjwD9mKQ7o72PaXxtHTJVUZdi8WAX9hCkmPygHdlPvfLj6krBpQmAZsgrdfSib0YX60AFJSBbdbrnfmn%2FcayOKmi3gCvaxIj0hE63zHkW6bvJAL7wI1d2ZdJbsbsJaHKpzF%2Bj%2F9ZwQP7cu%2BrMNmH1vXAAYE70qRXHGJmrI0961fsRwX%2BrIsC0vRwYdGAVDr8A7gzHa1CfMHm6cA%3D%3D)

- พอทำ Blue Green Deployment เสร็จ ReplicaSet: v1 จะยังคงเก็บไว้เพื่อเตรียม rolling back กลับไปได้ limit ที่ 10 revision history

## Deployment Strategy

1. RollingUpdate
	1. maxSurge
	2. maxUnavaliable
2. Re-create

# 3. Service


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/58ba9a1dadaca89cf603d190b86161ec.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=N0dBDgcpd%2BRG%2BOptbaOL%2Bgcuel5U8s7%2FKUD5u8oD7Ne%2F1iZHmGjSXW0BeTX7p4tzsHJz1oIuDiQxNE%2FOvj7bw8QA29AY%2B6EkTVNzRotJ9TSZrfpYDMME9RLmICz1Jc1ZYCUfeYuOqQ2IlmB0Jop%2FPUzGjHr4ikcGwt85bJrRy6GrYztZvXYRd9bkkhWp9%2BYKnz0FHY8iPgvEhdhCqu8ISZubXNOEzCWH7DQtoqEd87nfPtij1EqcWhQGgFJTKupQe4HNLJEiceB0xJ8Z6ApiR%2FrSSoH7ob0Fey3RkfCA5A8GlXipn%2BxXodbn8A5sSOSYTBD2HipzLukJLy4s1uTB%2Fg%3D%3D)

- Service มีหน้ที่ Forward Traffic จาก Load Balancer ไปยังแต่ละ Pod
- Load Balancer จะเป็น Virtual

# 4. Ingress


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a96aa9dabc8fc043117970d82b1a5286.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=nytu4oodsbQOmjsIHycObZX9D%2BhM3MldLRFAPOVK%2FfgwYqILSfofNO7t%2FR0Aqeof3B6SIr46vhk1x2PEFD0im5fxs20mU9Sj5mm%2FAYrQZP9uM8Sdlshu%2FTg4tJRbnlvfBfg0QSqOsSICb4iMHpzt3S3jMUjLJD7mrZ8YnPTun2eyHgApjROBtTxcMBWCYpJWvxbMBewQufebKdjtmJovdPlhzAO1VCEcnLhZcmj4OuYdg7s367ZPoBcoP8EFhjT8ktye0GNm%2BgC%2FPzpxHY8HnlFlEMxsxyin6HDCQlPv8iY5KBN1%2FK%2BELsFBs4ZWZrBFU5zr5%2BA0gpfS65YwKDCbJQ%3D%3D)

- Ingress เบื้องหลังสร้าง Load Balancer ขึ้นมาจริงๆ เป็น VM เบื้องหลัง
- สามารถ Mapping Domain ได้ (Mapping ผ่าน Domain ไปหา Service ที่เค้าต้องการได้)

# Power of Pod


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/8f2801547609b1279fd3049c3a24e7f9.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=dnOOo8d659%2FAwqVI7hXX70RZcaSG4%2B3EX6AZOCT2lg8NVXAIW0jcnRC7JUiECM0QWUsAc9UBLP6IpKPXEP1W%2BBgAipud5tLdFOaTMFj50DzBeGWDj%2BkXQ2c%2Bva8wUq2klqyum5yRuykIzDdiToXTs8uNLSJlhb7N%2FlitsE0Wq1HeIgJbEoydxLiBUnDE6jorG7tL%2BToScf1TiJimKx5Q7osrAJpbwO4HVFqpuofNR4BiZ8LFRIu%2Fl0%2F7nt4r9BaGiQkAgOAhMz0zJVeBlQPWWbfGP6qb15d39MEFJvEMeGyskn2ML%2FjuJ7eTI3gYCCezEp1re1HbXMgyeQrBorIyag%3D%3D)

- จะเห็นว่าแต่ละ components ของ Pod จะสามารถทำให้ Pod เพิ่ม Performance ได้

![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a4d2cc1006be723fd4c0983761a00cdc.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=ERxVpLi4HRtmuiYCS0ATkIjYZa9Et129zrWk1jDpP94fJ9EvO4nuKbEFus36b34EAnMobi3ydpmlZIMqyY%2BoQPbVo7Q2akrJvWvvY%2FdLT1ArSC6u9cKbst3fsZHoHYHXe0RIAT0OnGJ1lSrrMeI9cSBfMa50sqrAGs3zvTXzEvBo4rTsHrNRwdqil83b31ZUdqcMbhu90FmcGAN5jJpUw9Hs0VPpY7aL3UdXjWsCQ4fxhIzITFam%2B6WrbhX9n8h5g1STbQN9j2RzNKXGdE5vLVVqvwGeAxjcme%2FI4%2BK24nNNDhC1ObmLmKjm%2FlQaPs1UjYmFuVLvWr8nHo9w9wx03A%3D%3D)

- Flow ของ Kubernetes
