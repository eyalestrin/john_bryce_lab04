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
  **<code>kubectl run test-ns --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot17.jpg)  
14. Create a static pod on node01 called nginx-critical with image nginx. Create this pod on node01 and make sure that it is recreated/restarted automatically in case of a failure.  
  **<code>kubectl get nodes</code>**  
  **<code>kubectl label nodes ip-192-168-11-201.ec2.internal env=prod</code>**  
  Note: Replace the value of **ip-192-168-11-201.ec2.internal** according to the target node name.  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot18.jpg)  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot19.jpg)  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreateNginxCritical.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot20.jpg)  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot21.jpg)  
15. Create a pod called multi-pod with two containers.  
    Container 1: name: **alpha**, image: **nginx**, Environment Variable: **name: alpha**  
    Container 2: name: **beta**, image: **busybox**, command **sleep 4800**, Environment Variable: **name: beta**  
    **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreateMultiPod.yaml</code>**  
    ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot22.jpg)  
    ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot23.jpg)  
    ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot24.jpg)  
 
# Pod Design Questions:
1. Type the command for Get pods with label information:  
  **<code>kubectl get pods --show-labels</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot25.jpg)  
2. Create 5 nginx pods in which two of them is labeled **env=prod** and three of them is labeled **env=dev**  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreateMultiPodWithLabels.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot25.jpg)  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot26.jpg)  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot27.jpg)  
3. Change the label for one of the pod to **env=uat** and list all the pods to verify:  
  **<code>kubectl label --overwrite pods nginx5 env=uat</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot28.jpg)  
4. Remove the labels for the pods that we created now and verify all the labels are removed:  
  **<code>kubectl label pods nginx1 nginx2 nginx3 nginx4 nginx5 env-</code>**  
  **<code>kubectl get pods --show-labels</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot29.jpg)  
5. Add the label app=nginx for all the pods and verify:  
  **<code>kubectl label pods nginx1 nginx2 nginx3 nginx4 nginx5 app=nginx</code>**  
  **<code>kubectl get pods --show-labels</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot30.jpg)  
6. Get all the nodes with labels:  
  **<code>kubectl get nodes --show-labels</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot31.jpg)  
7. Label the worker node **nodeName=nginxnode**:  
  **<code>kubectl label nodes ip-192-168-11-201.ec2.internal ip-192-168-32-136.ec2.internal nodeName=nginxnode</code>**  
  Note: Update the node names according to the target nodes.  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot32.jpg)  
  **<code>kubectl get nodes --show-labels | grep nodeName=nginxnode</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot33.jpg)  
8. Create a Pod that will be deployed on the worker node with the label **nodeName=nginxnode**. Add the nodeSelector to the pod.  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/CreateNginxWithNodeSelector.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot34.jpg)  

# Deployments
1. Create a deployment called webapp with image nginx with 5 replicas:  
  * Use the below command to create a yaml file:  
    **<code>kubectl create deploy webapp --image=nginx --dry-run -o yaml > webapp.yaml</code>**  
  * Edit it and add 5 replica’s  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot35.jpg)  
2. Get the deployment rollout status and the replicaset that created with this deployment:  
  **<code>kubectl get deployments</code>**  
  **<code>kubectl get pods</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot36.jpg)  
3. Create a deployment of webapp with image nginx:1.17.1 with container port 80 and verify the image version.  
  **<code>kubectl create deploy webapp --image=nginx:1.17.1 --port=80 --dry-run=client -o yaml > webapp-upgrade.yaml</code>**  
  **<code>kubectl apply -f webapp-upgrade.yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot37.jpg)  
4. Update the deployment with the image version 1.17.4 and verify and check the rollout history and make sure everything is ok after the update:  
  **<code>kubectl set image deployment/webapp nginx=nginx:1.17.4</code>**  
  **<code>kubectl annotate --overwrite deployment webapp version='1.17.4'</code>**  
  **<code>kubectl rollout status deployment/webapp</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot38.jpg)  
5. Undo the deployment to the previous version 1.17.1 and verify Image has the previous version:  
  **<code>kubectl rollout history deployment/webapp</code>**  
  **<code>kubectl rollout undo deployment/webapp --to-revision=1</code>**  
  **<code>kubectl describe deployment webapp | grep Image</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot39.jpg)  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot40.jpg)  
6. Update the deployment with the wrong image version 1.100 and verify something is wrong with the deployment:  
  **<code>kubectl set image deployment/webapp nginx=nginx:1.100</code>**  
  **<code>kubectl get pods</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot41.jpg)  
7. Undo the deployment with the previous version and verify everything is Ok:  
  **<code>kubectl rollout history deployment/webapp</code>**  
  **<code>kubectl rollout history deploy webapp --revision=3</code>**  
  **<code>kubectl get deployment webapp</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot42.jpg)  
8. Update the deployment with the image version latest and check the history and verify nothing is going on:  
  **<code>kubectl set image deployment/webapp nginx=nginx:latest</code>**  
  **<code>kubectl rollout history deployment/webapp</code>**  
  **<code>kubectl describe deployment webapp | grep Image</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot43.jpg)  
9. Apply the autoscaling to this deployment with minimum 10 and maximum 20 replicas and target CPU of 85% and verify hpa is created and replicas are increased to 10 from 1  
  **<code>kubectl autoscale deployment webapp --cpu-percent=85 --min=10 --max=20</code>**  
  **<code>kubectl get hpa</code>**  
  **<code>kubectl get deployment</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot44.jpg)  
10. Clean the cluster by deleting deployment and hpa you just created.  
  **<code>kubectl delete deployment webapp</code>**  
  **<code>kubectl delete hpa webapp</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot45.jpg)  
11. Create a job and make it run 10 times one after one (run > exit > run >exit ..) using the following configuration:  
  **<code>kubectl create job hello-job --image=busybox --dry-run=client -o yaml -- echo "Hello I am from job" > hello-job.yaml</code>**  
  * Note: Add to the above job **completions: 10** inside the yaml  
  **<code>kubectl apply -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/hello-job.yaml</code>**  
  **<code>kubectl get pods</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot46.jpg)  

# CONFIG MAP
1. Create a file called config.txt with two values key1=value1 and key2=value2 and verify the file  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot47.jpg)  
2. Create a configmap named keyvalcfgmap and read data from the file config.txt and verify that configmap is created correctly.  
  **<code>kubectl create configmap keyvalcfgmap --from-file=config.txt</code>**  
  **<code>kubectl describe configmaps keyvalcfgmap</code>**  
  **<code>kubectl get configmap keyvalcfgmap -o yaml</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot48.jpg)  
3. Create an nginx pod and load environment values from the above configmap keyvalcfgmap and exec into the pod, verify the environment variables and delete the pod.  
  **<code>kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > nginx-pod.yml</code>**  
  **<code>kubectl create -f https://raw.githubusercontent.com/eyalestrin/john_bryce_lab04/main/yaml/nginx-pod.yml</code>**  
  **<code>kubectl exec -it nginx -- env | grep key</code>**  
  ![Alt text](https://github.com/eyalestrin/john_bryce_lab04/blob/main/images/screenshot49.jpg)  
