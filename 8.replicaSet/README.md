## Deploy ReplicaSet on Openshift/k8s

### Create replica-set
```bash
$ kubectl apply -f replicaSet.yml 
replicaset.apps/hello-rs created
```

### GEt replica-set
```bash
$ kubectl get rs 
NAME       DESIRED   CURRENT   READY   AGE
hello-rs   2         2         2       16s
```

### Describe replica-set
```bash
$ kubectl describe rs hello-rs 
Name:         hello-rs
Namespace:    replica-set
Selector:     app=hello,tier in (backend)
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"apps/v1","kind":"ReplicaSet","metadata":{"annotations":{},"name":"hello-rs","namespace":"replica-set"},"spec":{"replicas":2...
Replicas:     2 current / 2 desired
Pods Status:  2 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=hello
           tier=backend
  Containers:
   hello:
    Image:        openshift/hello-openshift
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age        From                   Message
  ----    ------            ----       ----                   -------
  Normal  SuccessfulCreate  <invalid>  replicaset-controller  Created pod: hello-rs-lpvv5
  Normal  SuccessfulCreate  <invalid>  replicaset-controller  Created pod: hello-rs-7vr79
```

### Scale replica-set to 5
```bash
$ kubectl scale rs hello-rs --replicas=5 
replicaset.extensions/hello-rs scaled
```

### Get REplica set
```bash
$ kubectl get rs 
NAME       DESIRED   CURRENT   READY   AGE
hello-rs   5         5         2       1m

$ kubectl get rs 
NAME       DESIRED   CURRENT   READY   AGE
hello-rs   5         5         4       1m

$ kubectl get rs 
NAME       DESIRED   CURRENT   READY   AGE
hello-rs   5         5         5       1m
```

### Get Pod
```bash
$ kubectl get pods 
NAME             READY   STATUS    RESTARTS   AGE
hello-rs-7vr79   1/1     Running   0          1m
hello-rs-ddw6x   1/1     Running   0          17s
hello-rs-k8ckz   1/1     Running   0          17s
hello-rs-lpvv5   1/1     Running   0          1m
hello-rs-zzdfs   1/1     Running   0          17s
```

### Create Service and display Yaml file using command
```bash
$ kubectl expose rs hello-rs --port=8080  --selector='app=hello' -o yaml 
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  name: hello-rs
spec:
  ports:
  - port: 8080
    protkubectlol: TCP
    targetPort: 8080
  selector:
    '''app': hello'
status:
  loadBalancer: {}
```

### Get Service
```bash
$ kubectl get svc 
NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
hello   ClusterIP   172.30.51.95   <none>        8080/TCP   49s
```

### check application using route url
```bash
$ curl <cluster-ip>:8080
Hello OpenShift!
```

### Delete Replica Set
```bash

$ kubectl delete rs hello-rs 
replicaset.extensions "hello-rs" deleted
```

### Delete All
```bash
$ kubectl delete all --all
```