# Amazon EKS cluster deployment
1. Install kubectl:  
  https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html  
2. Install eksctl:  
  https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html  
3. Create your Amazon EKS cluster and nodes (select "Managed nodes - Linux")  
  https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html#create-cluster-gs-eksctl

# Lab instructions:
1. Deploy a pod named nginx-pod using the nginx:alpine image.  
  'kubectl create deployment nginx-pod-yourname --image=nginx:alpine'
