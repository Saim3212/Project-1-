# $${\color{Blue} \textbf{Project}: \textbf{A} \ \}$$
### $\color{blue} \textbf{A project in integration of tools learnt throughout DevOps entirety \textbf . }$
> [!NOTE]
> The Flow Chart used will show a step by step tool usage. Commands will be provided along with explanations
<img width="817" height="654" alt="image" src="https://github.com/user-attachments/assets/5f56512e-4a4e-4932-b9e4-7e0e1249c086" />

The Project focusses on the CICD tools where The developer would initiate to push the code through to **Github **where by **Jenkins** would pull the source codes made by the developers. Jenkins would use **Maven** to compile the code and integrate the code into an artifact whereby the artifact will be into a **.JAR or .WAR** version of the file. The artifact is turned into a **Dockerfile** so it may be built into an image which is further pushed into** Dockerhub** to be pulled into a compiled and ready format.The **Kubernetes cludter** is thus created where we focus on creation of the **Master and the Worker Node** and focus is done towards an** EKS cluster** which is handled by **Pipeline** methods of **Jenkins**. 

> AWS Services and what tools within are to be used
<img width="546" height="413" alt="image" src="https://github.com/user-attachments/assets/7345e07c-bdff-4d8d-8e9f-4258d2486015" />

> Docker Services and what tools are to be used
<img width="610" height="468" alt="image" src="https://github.com/user-attachments/assets/7a0d7f14-a375-435c-8467-acab525d38a6" />


## $${\color{Red} \textbf{Jenkins Server Initiation} \ \}$$

We start by creating an EC2 instance with the following specifications 

An Amazon Linux Kernal 6 .12 AMI is chosen with an instance type of c7i flex large , a custom keypair is created along with Security group allowing ports to access later on and a config storage of 20 GB is chosen to be on the safe side 

<img width="583" height="704" alt="image" src="https://github.com/user-attachments/assets/918a6510-0818-4b50-8bf2-0a00a3c60ebe" />
<img width="580" height="504" alt="image" src="https://github.com/user-attachments/assets/81842744-688d-4309-b3fb-8e3a06c92fc9" />

Once the instance has been created , The following commands have been used as below 

````
sudo -i
yum update -y
yum upgrade -y
yum install java-21* -y
wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/rpm-stable/jenkins.repo
````
These commands install java 21 and install Jenkins repository so jenkins server may begin installation.
Once the Installation is complete 
````
systemctl start jenkins
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
yum install git -y 
````
<img width="614" height="253" alt="image" src="https://github.com/user-attachments/assets/08cd8a96-b56f-4b0e-b378-455ccdf495e9" />

To initiate Docker 
````
systemctl start docker
systemctl enable docker
````


