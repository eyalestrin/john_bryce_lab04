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
5. 
