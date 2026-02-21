# $${\color{Blue} \textbf{Project}: \textbf{A} \ \}$$
### $\color{blue} \textbf{A project in integration of tools learnt throughout DevOps entirety \textbf . }$
> [!NOTE]
> The Flow Chart used will show a step by step tool usage. Commands will be provided along with explanations
<img width="817" height="654" alt="image" src="https://github.com/user-attachments/assets/5f56512e-4a4e-4932-b9e4-7e0e1249c086" />

The Project focusses on the CICD tools where The developer would initiate to push the code through to **Github **where by **Jenkins** would pull the source codes made by the developers. Jenkins would use **Maven** to compile the code and integrate the code into an artifact whereby the artifact will be into a **.JAR or .WAR** version of the file. The artifact is turned into a **Dockerfile** so it may be built into an image which is further pushed into** Dockerhub** to be pulled into a compiled and ready format.The **Kubernetes cludter** is thus created where we focus on creation of the **Master and the Worker Node** and focus is done towards an** EKS cluster** which is handled by **Pipeline** methods of **Jenkins**. 

> AWS Services and what tools within are to be used
<img width="546" height="413" alt="image" src="https://github.com/user-attachments/assets/7345e07c-bdff-4d8d-8e9f-4258d2486015" />

> Docker Services and what tools are to be used
<img width="748" height="406" alt="image" src="https://github.com/user-attachments/assets/a5605e35-7d15-4b3f-90ad-4a4372437b49" />



## $${\color{Red} \textbf{Jenkins Server Initiation} \ \}$$

We start by creating an EC2 instance with the following specifications 

An Amazon Linux Kernal 6 .12 AMI is chosen with an instance type of c7i flex large , a custom keypair is created along with Security group allowing ports to access later on and a config storage of 20 GB is chosen to be on the safe side 

<img width="583" height="704" alt="image" src="https://github.com/user-attachments/assets/918a6510-0818-4b50-8bf2-0a00a3c60ebe" />
<img width="580" height="504" alt="image" src="https://github.com/user-attachments/assets/81842744-688d-4309-b3fb-8e3a06c92fc9" />

Once the instance has been created , The following commands have been used as below 

````
sudo -i
````
````
yum update -y
````
````
yum upgrade -y
````
````
yum install java-21* -y
````
````
wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/rpm-stable/jenkins.repo
````
````
yum install jenkins -y
````
These commands install java 21 and install Jenkins repository so jenkins server may begin installation.
Once the Installation is complete 
````
systemctl start jenkins
````
````
systemctl enable jenkins
````
Once the Jenkins has been initiated , we later go on to install Jenkins required dependencies and plugins 
which is done by pasting the instance Ip along with the prot 8080 
The password is asked which can be acquired by pasting the command in the instance.
````
cat /var/lib/jenkins/secrets/initialAdminPassword
````
<img width="637" height="68" alt="image" src="https://github.com/user-attachments/assets/a061e6cf-a3c5-4dc1-87ca-0e57fdeb3066" />

Plugin installations are asked which is default sleected as recommended , choose the first option and once installation is done
<img width="1051" height="531" alt="image" src="https://github.com/user-attachments/assets/a862478d-2b80-4dc2-b9d0-13f616fdd0ca" />

 Create the user as needed be 
 
<img width="933" height="519" alt="image" src="https://github.com/user-attachments/assets/f3257e76-e8d7-4d19-b9bf-c1853a9a50ab" />

Installations of docker and git is to be done now 


Commands are below 
````
yum install docker-io -y
````
````
yum install git -y 
````
<img width="614" height="253" alt="image" src="https://github.com/user-attachments/assets/08cd8a96-b56f-4b0e-b378-455ccdf495e9" />

To initiate Docker 
````
systemctl start docker
````
````
systemctl enable docker
````
Installation of kubectl can be done from the official Kuebrnetes website 
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
The command in use is 
````
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
````
Permission must be granted for kubectl
along with moving the files for the kubectl to work 
````
chmod +x kubectl
````
````
cp kubectl /usr/bin/k
````
````
cp kubectl /usr/bin
````
Maven installation 
````
yum install maven -y
````
## $${\color{Red} \textbf{IAM Roles creation} \ \}$$

<img width="753" height="453" alt="image" src="https://github.com/user-attachments/assets/8b3dae5d-dc52-4fe6-8754-ccd7c786b3f6" />

### $${\color{Orange} \textbf{MasterNode} \ \}$$

Choose EKS and select Cluster 
<img width="842" height="525" alt="image" src="https://github.com/user-attachments/assets/4f82f854-4edb-4932-a4d7-c76f39fe167c" />

Click next and name it MasterNode and select Next

### $${\color{Orange} \textbf{WorkerNode} \ \}$$

<img width="820" height="465" alt="image" src="https://github.com/user-attachments/assets/4171cde6-556b-471b-b28f-b7120cf1f9f3" />

Select EC2 at this stage and given below is the chosen permissions which must be allowed 
> [!NOTE]
> These are the Permissions that may be copied for easier navigation
````
AmazonEKS_CNI_Policy
````
````
AmazonEC2ContainerRegistryReadOnly
````
````
AmazonEKSWorkerNodePolicy
````
Once Done name the Role and click Next

<img width="1060" height="501" alt="image" src="https://github.com/user-attachments/assets/bc50fd7a-63b4-4227-b0ff-b9a07b932985" />

## $${\color{Red} \textbf{EKS Cluster creation} \ \}$$
Select EKS cluster and create a cluster

<img width="700" height="215" alt="image" src="https://github.com/user-attachments/assets/4b103863-dc72-4041-b84c-eb683eb992c5" />

Following Configs are to be used 

<img width="937" height="1069" alt="image" src="https://github.com/user-attachments/assets/f5b67e74-5ab9-4141-8320-a87e759cd4be" />

A security group has been created allowing all inbound and outbound rules for Project compatability purpouses 

<img width="957" height="1078" alt="image" src="https://github.com/user-attachments/assets/0f57d542-3ef3-4a73-92ac-920b913f7c2e" />

Click next and keep the rest as default and create the EKS cluster 
> [!IMPORTANT]
> Cluster Creation may take upto 15 minutes. 
<img width="971" height="756" alt="image" src="https://github.com/user-attachments/assets/8bf3976b-5059-4475-be29-3490db530a3d" />
