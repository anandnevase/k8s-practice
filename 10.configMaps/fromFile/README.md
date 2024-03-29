## Deploy Pod using config-map-from-file on Openshift/k8s

### Create  config-map from file
```bash
$ kubectl create configmap color-config-map-file --from-file=testfile.txt 
configmap/color-config-map-file created
```

### Get configMap config-map-from-file 
```bash
$ kubectl get configmap 
NAME                    DATA   AGE
color-config-map-file   1      16s
```

### Describe config-map-from-file
```bash
$ kubectl describe configmap color-config-map-file 
Name:         color-config-map-file
Namespace:    config-map-from-file
Labels:       <none>
Annotations:  <none>

Data
====
testfile.txt:
----
Hello  - ConfigMap from File
Events:  <none>
```

### Create POD
```bash
$ kubectl create -f pod.yml 
pod/color-app created
```

### Get POD
```bash
$ kubectl get pods 
NAME        READY   STATUS    RESTARTS   AGE
color-app   1/1     Running   0          8s
```

### Describe POD
```bash
$ kubectl describe pod color-app 
Name:         color-app
Namespace:    config-map-from-file
Priority:     0
Node:         master/172.17.0.12
Start Time:   Thu, 07 May 2020 08:38:17 +0530
Labels:       app=color-app
Annotations:  openshift.io/scc: anyuid
Status:       Running
IP:           10.128.0.25
IPs:          <none>
Containers:
  color-app:
    Container ID:   dkubectlker://25090a196c1ad94cdb640d5a2574fc6cb18bf7048f2e471fafb0517074ad6f7c
    Image:          anandnevase/color-app
    Image ID:       dkubectlker-pullable://dkubectlker.io/anandnevase/color-app@sha256:2e47fba350492f8d3c2417190cfcb4f9cac4b0e1733eafdd98d67a9ae52fe3cb
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 07 May 2020 08:38:22 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /data from config (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-x8n26 (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      color-config-map-file
    Optional:  false
  default-token-x8n26:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-x8n26
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  node-role.kubernetes.io/compute=true
Tolerations:     <none>
Events:
  Type    Reason     Age        From               Message
  ----    ------     ----       ----               -------
  Normal  Scheduled  <invalid>  default-scheduler  Successfully assigned config-map-from-file/color-app to master
  Normal  Pulling    <invalid>  kubelet, master    pulling image "anandnevase/color-app"
  Normal  Pulled     <invalid>  kubelet, master    Successfully pulled image "anandnevase/color-app"
  Normal  Created    <invalid>  kubelet, master    Created container
  Normal  Started    <invalid>  kubelet, master    Started container
```

### create service
```bash
$ kubectl expose pod/color-app  --type=NodePort
service/color-app exposed
```

###  Accees application
Access application using http://minikube-ip:nodeport/read_file

### Delete All
```bash
$ kubectl delete secrets color-secret
$ kubectl delete all --all
```