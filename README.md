# MetalLB on Minikube (quick start)
Quick example on how to test metalLB on Minikube.  
I've tested this environment on:
 - Ubuntu 18.04.3 LTS
 - metallb v0.8.1
 - minikube version: v1.4.0
 - k8s:
   - Client Version: v1.16.2
   - Server Version: v1.16.0

## Installation
Install MetalLB:
``` bash
$ kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.1/manifests/metallb.yaml
```

## Configure 
Configure the loadbalancing service via configmaps:
``` bash
kubectl apply -f metalLB-configmap.yaml
```

## Test
Deploy some pods and expose the services via metalLB
``` bash
$ kubectl apply -f deployment1.yaml
$ kubectl apply -f deployment2.yaml
$ kubectl apply -f hello-minikube.yaml
``` 
``` bash
$ kubectl get svc
minik8-service   LoadBalancer   10.111.104.80   192.168.99.113   8080:30368/TCP   21s
my-service       LoadBalancer   10.98.166.32    192.168.99.111   80:32739/TCP     37s
my-service2      LoadBalancer   10.98.5.175     192.168.99.112   80:31914/TCP     29s
``` 
Check if the services are reachable:
``` bash
$ wget 192.168.99.111
  --2019-10-25 13:31:40--  http://192.168.99.111/
  Connecting to 192.168.99.111:80... connected.
  HTTP request sent, awaiting response... 200 OK
  Length: 612 [text/html]
  Saving to: ‘index.html.1’
  
  index.html.1                       100%[==================================>]     612  --.-KB/s    in 0s      

$ wget 192.168.99.113:8080
  --2019-10-25 13:33:21--  http://192.168.99.113:8080/
  Connecting to 192.168.99.113:8080... connected.
  HTTP request sent, awaiting response... 200 OK
  Length: unspecified [text/plain]
  Saving to: ‘index.html’
  
  index.html                             [ <=>                               ]     356  --.-KB/s    in 0.001s  
  2019-10-25 13:33:21 (419 KB/s) - ‘index.html’ saved [356]
``` 

## Notes
If you want to exeplore how it does work, login into the minikube machine, and check the iptables NAT configurations:
```bash
minikube ssh
$ sudo iptables -L -t nat
```

