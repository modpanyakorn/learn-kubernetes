
# Command

- เราสามารถที่จะ mapping port ไปที่ target components ที่เราต้องการได้โดยระบุหลังตำแหน่ง@

```bash
k3d cluster create my-k3d \
	--servers 1 \
	--agents 3 \
	--port "8888:80@loadbalancer" \
	--port "8889:443@server:0"
```

- @server:<INDEX> คือ mapping port ไปที่ server container
- @loadbalancer:<INDEX> คือ mapping port ไปที่ load balancer
- @agent:<INDEX> คือ mapping port ไปที่ worker node container

## `kubectl` 


เป็น command ไว้คุยกับ API Server ทำหน้าที่ใจการจัดการ Cluster “kubectl controls the Kubernetes cluster manager.”

- `create` สร้าง resource เช่น Pod, Service, Deployment จาก input ที่ป้อนเข้ามาหรือ yaml/json
- `get` show resource เช่น `pods, services, all`

	```bash
	# get all
	$ kubectl get all
	NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
	service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   45m
	
	# get node
	$ kubectl get node
	NAME                  STATUS   ROLES                  AGE   VERSION
	k3d-my-k3d-agent-0    Ready    <none>                 49m   v1.31.5+k3s1
	k3d-my-k3d-agent-1    Ready    <none>                 49m   v1.31.5+k3s1
	k3d-my-k3d-agent-2    Ready    <none>                 49m   v1.31.5+k3s1
	k3d-my-k3d-server-0   Ready    control-plane,master   49m   v1.31.5+k3s1
	
	# more informatiom
	$ kubectl get node -o wide
	NAME                  STATUS   ROLES                  AGE    VERSION        INTERNAL-IP   EXTERNAL-IP   OS-IMAGE           KERNEL-VERSION                     CONTAINER-RUNTIME
	k3d-my-k3d-agent-0    Ready    <none>                 113m   v1.31.5+k3s1   172.18.0.6    <none>        K3s v1.31.5+k3s1   6.6.87.2-microsoft-standard-WSL2   containerd://1.7.23-k3s2
	k3d-my-k3d-agent-1    Ready    <none>                 113m   v1.31.5+k3s1   172.18.0.5    <none>        K3s v1.31.5+k3s1   6.6.87.2-microsoft-standard-WSL2   containerd://1.7.23-k3s2
	k3d-my-k3d-agent-2    Ready    <none>                 113m   v1.31.5+k3s1   172.18.0.4    <none>        K3s v1.31.5+k3s1   6.6.87.2-microsoft-standard-WSL2   containerd://1.7.23-k3s2
	k3d-my-k3d-server-0   Ready    control-plane,master   113m   v1.31.5+k3s1   172.18.0.3    <none>        K3s v1.31.5+k3s1   6.6.87.2-microsoft-standard-WSL2   containerd://1.7.23-k3s2
	```

- `api-resources` ดูว่า มี object อะไรบ้าง

	```bash
	$ kubectl api-resources
	NAME                                SHORTNAMES   APIVERSION                        NAMESPACED   KIND
	bindings                                         v1                                true         Binding
	componentstatuses                   cs           v1                                false        ComponentStatus
	configmaps                          cm           v1                                true         ConfigMap
	endpoints                           ep           v1                                true         Endpoints
	events                              ev           v1                                true         Event
	limitranges                         limits       v1                                true         LimitRange
	namespaces                          ns           v1                                false        Namespace
	nodes                               no           v1                                false        Node
	persistentvolumeclaims              pvc          v1                                true         PersistentVolumeClaim
	persistentvolumes                   pv           v1                                false        PersistentVolume
	pods                                po           v1                                true         Pod
	podtemplates                                     v1                                true         PodTemplate
	replicationcontrollers              rc           v1                                true         ReplicationController
	resourcequotas                      quota        v1                                true         ResourceQuota
	secrets                                          v1                                true         Secret
	serviceaccounts                     sa           v1                                true         ServiceAccount
	services                            svc          v1                                true         Service
	mutatingwebhookconfigurations                    admissionregistration.k8s.io/v1   false        MutatingWebhookConfiguration
	validatingadmissionpolicies                      admissionregistration.k8s.io/v1   false        ValidatingAdmissionPolicy
	validatingadmissionpolicybindings                admissionregistration.k8s.io/v1   false        ValidatingAdmissionPolicyBinding
	validatingwebhookconfigurations                  admissionregistration.k8s.io/v1   false        ValidatingWebhookConfiguration
	customresourcedefinitions           crd,crds     apiextensions.k8s.io/v1           false        CustomResourceDefinition
	apiservices                                      apiregistration.k8s.io/v1         false        APIService
	controllerrevisions                              apps/v1                           true         ControllerRevision
	daemonsets                          ds           apps/v1                           true         DaemonSet
	deployments                         deploy       apps/v1                           true         Deployment
	replicasets                         rs           apps/v1                           true         ReplicaSet
	statefulsets                        sts          apps/v1                           true         StatefulSet
	selfsubjectreviews                               authentication.k8s.io/v1          false        SelfSubjectReview
	tokenreviews                                     authentication.k8s.io/v1          false        TokenReview
	localsubjectaccessreviews                        authorization.k8s.io/v1           true         LocalSubjectAccessReview
	selfsubjectaccessreviews                         authorization.k8s.io/v1           false        SelfSubjectAccessReview
	selfsubjectrulesreviews                          authorization.k8s.io/v1           false        SelfSubjectRulesReview
	subjectaccessreviews                             authorization.k8s.io/v1           false        SubjectAccessReview
	horizontalpodautoscalers            hpa          autoscaling/v2                    true         HorizontalPodAutoscaler
	cronjobs                            cj           batch/v1                          true         CronJob
	jobs                                             batch/v1                          true         Job
	certificatesigningrequests          csr          certificates.k8s.io/v1            false        CertificateSigningRequest
	leases                                           coordination.k8s.io/v1            true         Lease
	endpointslices                                   discovery.k8s.io/v1               true         EndpointSlice
	events                              ev           events.k8s.io/v1                  true         Event
	flowschemas                                      flowcontrol.apiserver.k8s.io/v1   false        FlowSchema
	prioritylevelconfigurations                      flowcontrol.apiserver.k8s.io/v1   false        PriorityLevelConfiguration
	helmchartconfigs                                 helm.cattle.io/v1                 true         HelmChartConfig
	helmcharts                                       helm.cattle.io/v1                 true         HelmChart
	addons                                           k3s.cattle.io/v1                  true         Addon
	etcdsnapshotfiles                                k3s.cattle.io/v1                  false        ETCDSnapshotFile
	nodes                                            metrics.k8s.io/v1beta1            false        NodeMetrics
	pods                                             metrics.k8s.io/v1beta1            true         PodMetrics
	ingressclasses                                   networking.k8s.io/v1              false        IngressClass
	ingresses                           ing          networking.k8s.io/v1              true         Ingress
	networkpolicies                     netpol       networking.k8s.io/v1              true         NetworkPolicy
	runtimeclasses                                   node.k8s.io/v1                    false        RuntimeClass
	poddisruptionbudgets                pdb          policy/v1                         true         PodDisruptionBudget
	clusterrolebindings                              rbac.authorization.k8s.io/v1      false        ClusterRoleBinding
	clusterroles                                     rbac.authorization.k8s.io/v1      false        ClusterRole
	rolebindings                                     rbac.authorization.k8s.io/v1      true         RoleBinding
	roles                                            rbac.authorization.k8s.io/v1      true         Role
	priorityclasses                     pc           scheduling.k8s.io/v1              false        PriorityClass
	csidrivers                                       storage.k8s.io/v1                 false        CSIDriver
	csinodes                                         storage.k8s.io/v1                 false        CSINode
	csistoragecapacities                             storage.k8s.io/v1                 true         CSIStorageCapacity
	storageclasses                      sc           storage.k8s.io/v1                 false        StorageClass
	volumeattachments                                storage.k8s.io/v1                 false        VolumeAttachment
	ingressroutes                                    traefik.containo.us/v1alpha1      true         IngressRoute
	ingressroutetcps                                 traefik.containo.us/v1alpha1      true         IngressRouteTCP
	ingressrouteudps                                 traefik.containo.us/v1alpha1      true         IngressRouteUDP
	middlewares                                      traefik.containo.us/v1alpha1      true         Middleware
	middlewaretcps                                   traefik.containo.us/v1alpha1      true         MiddlewareTCP
	serverstransports                                traefik.containo.us/v1alpha1      true         ServersTransport
	tlsoptions                                       traefik.containo.us/v1alpha1      true         TLSOption
	tlsstores                                        traefik.containo.us/v1alpha1      true         TLSStore
	traefikservices                                  traefik.containo.us/v1alpha1      true         TraefikService
	ingressroutes                                    traefik.io/v1alpha1               true         IngressRoute
	ingressroutetcps                                 traefik.io/v1alpha1               true         IngressRouteTCP
	ingressrouteudps                                 traefik.io/v1alpha1               true         IngressRouteUDP
	middlewares                                      traefik.io/v1alpha1               true         Middleware
	middlewaretcps                                   traefik.io/v1alpha1               true         MiddlewareTCP
	serverstransports                                traefik.io/v1alpha1               true         ServersTransport
	serverstransporttcps                             traefik.io/v1alpha1               true         ServersTransportTCP
	tlsoptions                                       traefik.io/v1alpha1               true         TLSOption
	tlsstores                                        traefik.io/v1alpha1               true         TLSStore
	traefikservices                                  traefik.io/v1alpha1               true         TraefikService
	```


# Create Pods


ปกจิเราจะไม่สร้าง Pods ขึ้นมาเดี่ยว

- `kubectl run nginx --image=nginx`
- เมื่อเรารันคำสั่ง `kubectl run` จะถูกส่งไปยัง API Server แล้วจะสร้างเป็น Pod

## Show Pod


```bash
$ kubectl get pod
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          56s
```


## Describe


```bash
$ kubectl describe pod
Name:             nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             k3d-my-k3d-server-0/172.18.0.3
Start Time:       Wed, 26 Nov 2025 14:02:57 +0700
Labels:           run=nginx
Annotations:      <none>
Status:           Running
IP:               10.42.3.4
IPs:
  IP:  10.42.3.4
Containers:
  nginx:
    Container ID:   containerd://24a5af2dec72f12fdfe865e61457f1503c4ee10ddc81c0f969bd2ea4ac05768c
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:553f64aecdc31b5bf944521731cd70e35da4faed96b2b7548a3d8e2598c52a42
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 26 Nov 2025 14:03:14 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-db5rc (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-db5rc:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>
```


## Get Pod Information


```bash
$ kubectl get pod -o wide # -o wide จะบอกรายละเอียดเพิ่มเติมได้่
NAME    READY   STATUS    RESTARTS   AGE   IP          NODE                  NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          91s   10.42.3.4   k3d-my-k3d-server-0   <none>           <none>
```


## Show logs


```bash
$ kubectl logs nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/11/26 07:03:16 [notice] 1#1: using the "epoll" event method
2025/11/26 07:03:16 [notice] 1#1: nginx/1.29.3
2025/11/26 07:03:16 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2025/11/26 07:03:16 [notice] 1#1: OS: Linux 6.6.87.2-microsoft-standard-WSL2
2025/11/26 07:03:16 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/11/26 07:03:16 [notice] 1#1: start worker processes
2025/11/26 07:03:16 [notice] 1#1: start worker process 29
2025/11/26 07:03:16 [notice] 1#1: start worker process 30
2025/11/26 07:03:16 [notice] 1#1: start worker process 31
2025/11/26 07:03:16 [notice] 1#1: start worker process 32
2025/11/26 07:03:16 [notice] 1#1: start worker process 33
2025/11/26 07:03:16 [notice] 1#1: start worker process 34
2025/11/26 07:03:16 [notice] 1#1: start worker process 35
2025/11/26 07:03:16 [notice] 1#1: start worker process 36
```


## Edit Pod


```bash
$ kubectl edit pod <POD-NAME>
```


ทดลอง เปลี่ยน version image


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/aa38d6491208c7d25d118e65a162b56f.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=iBoslk5nfCswS4PhESRe9QWj3imOJBJbLCWs8pz6UdbljJk6t0C4SKAAOn%2FPizgGwbRtxuOezQR3S8tJSSaAcvw%2FDZfa0FqkcVKYL1jONtuYpdbo719vYCCdF0Jbva%2BpsmPu%2Feum%2F%2FTjcPE8VwF5TZDObG6YcgPmM%2FcGgUeFzw07BxBt8RDV8tEAWwsWmge510JmDs9kdgSwzwJsDHN39p3OgMjGKNahdjU4rDsEkTAzm7arUBTpli2KvEW8He52Pn7GEalJcWHT4dxORLVndNt4zk6FjoSHMoTkvSthT5L7ASg%2BOLxnY29BmuNQ4F7oJFAfS5YsJ1mA5ia9sqeh3w%3D%3D)


![image.png](https://storage.googleapis.com/dashboard-51ba6.appspot.com/a1fedf68dd5881a5944996e42d26d992.png?GoogleAccessId=firebase-adminsdk-jd298%40dashboard-51ba6.iam.gserviceaccount.com&Expires=16725225600&Signature=dcM353d8Ri1kgejYMHETRNxPti%2F3TevRmuZVB7vGoIDghvc%2FqytKlv8s5WKZYP8iK5Dr9J4yhFkHm8r93NE1JuNj9WmCZbYsaeXKQT4eZkoQhCBQsk0eFNv%2FZoG4i2pHFJUNfKu3IkdAObcwKiJjFnIf%2BVlgAin5uIeL0zAmbPWksSq2idS71Lbmq%2BwDPxAFZa%2FfznFG%2FZmb9%2FmFxvxHbfko9fgdjq3VShlRyqGjjPqiDi39K3REtdTikE%2FwSYcElbti1au81dLnYWsxpp26PB6UIc9vPkxFMuuriiMDyAX7I8HQaY68%2BYy36DXCZMK3v9sKkmQeNdEfArLwVeaAPA%3D%3D)


จะเห็นว่าตัวของ Pod จะพึ่ง Restart ไป


```bash
Events:
  Type     Reason   Age                    From     Message
  ----     ------   ----                   ----     -------
  Normal   Pulling  10m                    kubelet  Pulling image "nginx:1.21.6"
  Normal   Pulled   9m47s                  kubelet  Successfully pulled image "nginx:1.21.6" in 16.302s (16.302s including waiting). Image size: 56746739 bytes.
  Normal   Killing  3m32s (x2 over 10m)    kubelet  Container nginx definition changed, will be restarted
  Normal   BackOff  3m3s                   kubelet  Back-off pulling image "nginx:1.21.7"
  Warning  Failed   3m3s                   kubelet  Error: ImagePullBackOff
  Normal   Pulling  2m51s (x3 over 3m32s)  kubelet  Pulling image "nginx:1.21.7"
  Warning  Failed   2m49s (x3 over 3m30s)  kubelet  Failed to pull image "nginx:1.21.7": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/library/nginx:1.21.7": failed to resolve reference "docker.io/library/nginx:1.21.7": docker.io/library/nginx:1.21.7: not found
  Warning  Failed   2m49s (x3 over 3m30s)  kubelet  Error: ErrImagePull
  Warning  BackOff  2m21s (x3 over 3m30s)  kubelet  Back-off restarting failed container nginx in pod nginx_default(7519b160-0ead-408f-b5ca-8445ceb78e8d)
  Normal   Pulling  2m14s                  kubelet  Pulling image "nginx:1.29"
  Normal   Started  2m12s (x3 over 3h38m)  kubelet  Started container nginx
  Normal   Created  2m12s (x3 over 3h38m)  kubelet  Created container nginx
  Normal   Pulled   2m12s                  kubelet  Successfully pulled image "nginx:1.29" in 1.855s (1.855s including waiting). Image size: 59772801 bytes.
```

- จาก Event logs ก็จะเห็นว่าตัวของ kubelet ได้มีการ pull image version ใหม่มาแทนตัวเดิม

เช็ค image version จาก `kubectl get pod nginx -o json`


```bash
$ kubectl get pod nginx -o json
{
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
        "creationTimestamp": "2025-11-26T07:02:57Z",
        "labels": {
            "run": "nginx"
        },
        "name": "nginx",
        "namespace": "default",
        "resourceVersion": "14908",
        "uid": "7519b160-0ead-408f-b5ca-8445ceb78e8d"
    },
    "spec": {
        "containers": [
            {
                "image": "nginx:1.29",
                "imagePullPolicy": "Always",
                "name": "nginx",
                "resources": {},
                "terminationMessagePath": "/dev/termination-log",
                "terminationMessagePolicy": "File",
                "volumeMounts": [
```


### ทดลองใช้ Ephermeral Image (Image ที่มีการทำงานชั่วคราว)


```bash
$ kubectl get pod -o wide
NAME          READY   STATUS             RESTARTS       AGE     IP          NODE                  NOMINATED NODE   READINESS GATES
hello-world   0/1     CrashLoopBackOff   7 (115s ago)   13m     10.42.2.4   k3d-my-k3d-agent-2    <none>           <none>
```

- จะเห็นว่า STATUS: CrashLoopBackOff เพราะว่าตัวของ Container มัน Start แค่ทำงานเสร็จตาม Logic

```bash
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  14m                   default-scheduler  Successfully assigned default/hello-world to k3d-my-k3d-agent-2
  Normal   Pulled     14m                   kubelet            Successfully pulled image "hello-world" in 5.698s (5.698s including waiting). Image size: 16303 bytes.
  Normal   Pulled     14m                   kubelet            Successfully pulled image "hello-world" in 1.819s (1.819s including waiting). Image size: 16303 bytes.
  Normal   Pulled     14m                   kubelet            Successfully pulled image "hello-world" in 1.859s (1.86s including waiting). Image size: 16303 bytes.
  Normal   Created    13m (x4 over 14m)     kubelet            Created container hello-world
  Normal   Started    13m (x4 over 14m)     kubelet            Started container hello-world
  Normal   Pulled     13m                   kubelet            Successfully pulled image "hello-world" in 1.779s (1.779s including waiting). Image size: 16303 bytes.
  Normal   Pulling    13m (x5 over 14m)     kubelet            Pulling image "hello-world"
  Normal   Pulled     13m                   kubelet            Successfully pulled image "hello-world" in 1.734s (1.734s including waiting). Image size: 16303 bytes.
  Warning  BackOff    3m45s (x46 over 14m)  kubelet            Back-off restarting failed container hello-world in pod hello-world_default(1f63433a-e798-4b53-b2
```


```bash
$ kubectl get pod -o wide
NAME          READY   STATUS             RESTARTS       AGE     IP          NODE                  NOMINATED NODE   READINESS GATES
hello-world   0/1     CrashLoopBackOff   7 (5m1s ago)   16m     10.42.2.4   k3d-my-k3d-agent-2    <none>           <none>
nginx         1/1     Running            2 (44m ago)    4h20m   10.42.3.4   k3d-my-k3d-server-0   <none> 
```

- จะเห็นว่ามีการ Start Container ใหม่ (Successfully) อยู่หลายครั้ง และ RESTARTS 7 ครั้งเลยทำให้เกิด STATUS: CrashLoopBackOff
- เราสามารถ Delete Pods ที่ไม่ใช้ทิ้งได้

```bash
$ kubectl delete pod hello-world
pod "hello-world" deleted from default namespace
```

