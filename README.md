# SampleMERNwithMicroservices

## Prerequisites
1: Launch an EC2 (min of 4Gb ram and 20 GB disk)

2: Install aws-cli in it

3: Install Jenkins in it

4: Install docker in it

5: Install EKSCTL in it

6: Install Kubectl in it

## Set Up the AWS Environment

1. Set Up AWS CLI and Boto3:
   - Install AWS CLI and configure it with AWS credentials.
     
```
aws configure
AWS Access Key ID [None]: ***************
AWS Secret Access Key [None]: ***************
Default region name [None]: ap-south-1
Default output format [None]:
```
   - Install Boto3 for Python and configure it.

## Prepare the MERN Application

1. Containerize the MERN Application:

   - Ensure the MERN application is containerized using Docker. Create a Dockerfile for each component (frontend and backend).

     Backend Microservice
     https://github.com/TeamKanyarasi/MERN_App_Microservices/blob/main/backend/README.md

     Frontend Microservice
     https://github.com/TeamKanyarasi/MERN_App_Microservices/blob/main/frontend/README.md

2. Push Docker Images to Amazon ECR:

   - Build Docker images for the frontend and backend.

   - Create an Amazon ECR repository for each image.

   - Push the Docker images to their respective ECR repositories.
  
![Screenshot (140)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/22dd7b5b-a80e-47b4-bb89-db22dbfe5f85)

![Screenshot (141)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/08b2d0ad-ba65-4c97-b1e5-fa4d8c31bd9e)

![Screenshot (142)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/d072508b-3ddd-486b-a5bd-a48dd3ae57d6)


## Version Control

1. Use AWS CodeCommit:

   - Create a CodeCommit repository.
   
      Install the git-remote-codecommit in the EC2

```
apt install python3-pip
pip install git-remote-codecommit
``` 
```
 git clone codecommit::ap-south-1://<repo-name>
```
   - Push the MERN application source code to the CodeCommit repository.

![Screenshot (143)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/285ec8b3-bf01-47ea-aff4-c1039b2765cb)



## Kubernetes (EKS) Deployment

1. Create EKS Cluster:

   - Use eksctl or other tools to create an Amazon EKS cluster.

     Installing EKSCTL on EC2 using https://eksctl.io/installation/
    ```
    # for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`
   ARCH=amd64
   PLATFORM=$(uname -s)_$ARCH
   
   curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"
   
   # (Optional) Verify checksum
   curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check
   
   tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz
   sudo mv /tmp/eksctl /usr/local/bin
    ```
    ```
    eksctl create cluster --name <your-cluster-name> --region <region-name> --nodegroup-name standard-workers --node-type t2.micro --nodes 2 --nodes-min 1 --nodes-max 3
    ```
    
    - Install kubectl to access the cluster  [https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html]
      
     ```
     curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl
     curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl.sha256
     sha256sum -c kubectl.sha256
     chmod +x ./kubectl
     mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
     echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
     kubectl version --client
     ```
     ```
     aws eks --region <region-name> update-kubeconfig --name <your-cluster-name>
     ```
    Pass the env variables into the deployment.yaml file using the below command
   ```
   kubectl create secret generic mongo-secret --from-literal=MONGO_URL=" your mongo db url"
   ```
    Apply all the deployment.yaml files from the following link using the below command
   
    https://github.com/TeamKanyarasi/MERN_App_Microservices/tree/main/deployment

    ```
    kubectl apply -f <deployment-file-name.yml>
    ```
    Additional commad
    ```
    eksctl create nodegroup --cluster <cluster-name> --region <region> --name <nodegroup-name> --node-type <node-type> --nodes <number-of-nodes>
    ```
    
![Screenshot (129)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/efe26be3-b6d5-4d29-b26d-000a5000a981)

![Screenshot (130)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/9c5c0e83-c5cd-472e-9765-f496a636049b)

Pass the loadbalancer urls of the both the backend services into the frontend deployment.yaml file as env variables.

![Screenshot (131)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/feb34f2a-7a82-4a17-8d9a-f016f488033b)

![Screenshot (132)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/37660548-2648-448f-bb6a-84187e2eb417)

![Screenshot (133)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/512a829b-53e4-45e5-9191-c78187dcba46)


3. Deploy Application with Helm:

   - Use Helm to package and deploy the MERN application on EKS.


Step 11: Monitoring and Logging

1. Set Up Monitoring:

   - Use CloudWatch for monitoring and setting up alarms.
   - Use  Amazon CloudWatch Observability add-on from the Add-ons section in the cluster.

![Screenshot (136)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/adff7172-ae7f-4567-9ccf-3350c637d834)

   - CloudWatch Alarm for monitoring and notification services.

 ![Screenshot (138)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/8c4d8967-4fe0-4fea-abe4-e95eca47a92f)

 ![Screenshot (137)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/e519bb23-5417-40c7-beef-78567e8230c9)

 ![Screenshot (139)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/f251050e-4576-4ddd-a6a9-66a398b7727e)


2. Configure Logging:

   - Use CloudWatch Logs or another logging solution for collecting logs.

![Screenshot (144)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/ccaff468-07f9-4e20-a49d-d17aabfc95c8)
