# Amazon EKS cluster deployment
1. Install kubectl:  
  https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html  
2. Install eksctl:  
  https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html  
3. Create your Amazon EKS cluster and nodes (select "Managed nodes - Linux")  
  https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html#create-cluster-gs-eksctl

# Lab instructions:
1. Deploy a pod named nginx-pod using the nginx:alpine image.  
  **<code>kubectl create deployment nginx-pod-yourname --image=nginx:alpine</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot01.jpg)
2. Deploy a messaging pod using the redis:alpine image with the labels set to tier=msg.  
  **<code>kubectl create deployment messaging --image=redis:alpine</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot02.jpg)  
  **<code>kubectl get pods -o custom-columns=NAME:.metadata.name,PHASE:.status.phase,LABELS:.metadata.labels -l tier</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot03.jpg)
3. Create a namespace named apx-x998.  
  **<code>kubectl create namespace apx-x998</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot04.jpg)
4. Get the list of nodes in JSON format and store it in a file at /tmp/nodes.txt  
  **<code>kubectl get nodes --output=json > /tmp/nodes.txt</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot05.jpg)
5. Create a service messaging-service to expose the messaging application within the cluster on port 6379.  
  **<code>kubectl create service clusterip messaging-service --tcp=6379:6379</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot06.jpg)
6. Create a deployment named hr-web-app using the image kodekloud/webapp-color with 2 replicas.  
  **<code>kubectl create deployment hr-web-app --image=kodekloud/webapp-color --replicas 2</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot07.jpg)
7. Create a static pod named static-busybox on the master node that uses the busybox image and the command sleep 1000.  
  **<code>kubectl run static-busybox --image=busybox --restart=Never – /bin/sh ‘sleep 1000’</code>**
8. Create a POD in the finance namespace named temp-bus with the image redis:alpine  
  **<code>kubectl create namespace finance</code>**  
  **<code>kubectl run temp-bus --image=redis:alpine --namespace=finance</code>**  
  **<code>kubectl get pods --namespace=finance</code>**  
  ![Alt text](https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/master/images/screenshot08.jpg)
9. Create a Persistent Volume with the given specification:  
  * Volume Name: pv-analytics  
  * Storage: 100Mi  
  * Access modes: ReadWriteMany  
  * Host Path: /pv/data-analytics  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreatePersistentStorage.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/99f1c26de06b1a650685448da169ad9401f6f09d/images/screenshot09.jpg)
10. Create a Pod called redis-storage with image: redis:alpine with a Volume of type emptyDir that lasts for the life of the Pod. specs:.  
  * Pod named 'redis-storage'  
  * Pod 'redis-storage' uses Volume type of emptyDir  
  * Pod 'redis-storage' uses volumeMount with mountPath = /data/redis  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreateRedisEmptyDir.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/2a1f2d191f01795783bc9eb8a52d7e96c6d71c2a/images/screenshot10.jpg)
11. Create this pod and attached it a persistent volume called pv-1. Make sure the PV mountPath is hostbase : /data  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot11.jpg)  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreateNginxVolume.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot12.jpg)  
12. Create a new deployment called nginx-deploy, with image nginx:1.16 and 1 replica and record the version.  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/nginx-deploy116.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot13.jpg)  
  Upgrade the deployment to version 1.17 using rolling update. Make sure that the version upgrade is recorded in the resource annotation.  
  **<code>kubectl set image deployment/nginx-deploy nginx=nginx:1.17</code>**  
  **<code>kubectl annotate --overwrite deployment nginx-deploy version='1.17'</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot14.jpg)  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot15.jpg)  
13. Create an nginx pod called nginx-resolver using image nginx, expose it internally with a service called nginx-resolver-service. Test that you are able to look up the service and pod names from within the cluster. Use the image: busybox:1.28 for dns lookup. Record results in $HOME/nginx.txt  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreateNginxResolver.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot16.jpg)  
