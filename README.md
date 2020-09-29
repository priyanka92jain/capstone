# capstone
Capstone project

Git HUb repo : https://github.com/priyanka92jain/capstone

1) Github repo contain following 


 - Jenkinsfile   
 - Dockerfile 
 - index.html - Code used in docker 
 - cluster networking.yml  (created by eksctl)
 - cluster worker nodes.yml (created by eksctl)
 - deployment manifest 
 - servcies manifest 


2) eksctl automatically created all networking related to a kubernetes cluster and worker nodes via cloud formation. Yaml used by same is copied and is added to git hub repo. 


3) Screenshots
1. docker-Image-ecr.PNG
2. EC2-instance using cluster Node
3. ECR-Capstone
4. EKSClusterDetails
5. GreenBlueDeployment-Branch-development
6. GreenBlueDeployment-Branch-master
7. Jenkins-Successful-Pipeline
8. Lint Error
9. S3-bucket-static-priyankajain-jenkins
10. UploadAWS-staticContent


Capstone : I have used eksctl to create kubernetes cluster using below steps
1) Install aws cli 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt-get update 
sudo apt-get install unzip 
unzip awscliv2.zip
sudo ./aws/install

2) Configure aws cli 

3) Install docker 
sudo apt-get install docker.io

Creat a docker file in current direcotry with below parameters 

FROM nginx:alpine
COPY . index.html /usr/share/nginx/html/

4) Make docker image 
docker build -t my-app .

5) Run docker image 
docker run -p 8000:80 my-app 

6) Check the container 
ec2 machine IP:8000

7) Create ecr registry and upload image 
To deal with access issues run below command 
sudo usermod -a -G docker $USER
newgrp docker

8) Install  eksctl (https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

9) Install kuvbectl 
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.9/2020-08-04/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
echo 'export PATH=$PATH:$HOME/bin' >> ~/.bash_profile
kubectl version --short --client


10) Create a cluster 
eksctl create cluster --name priyanka-cluster --version 1.17 --region us-west-2 --nodegroup-name priyanka-nodes --node-type t3.small --ssh-access --ssh-public-key priyankacluster --managed

eksctl create cluster --name priyanka-cluster --version 1.17 --region us-east-2 --nodegroup-name priyanka-nodes --node-type t3.small --nodes-min 1 --nodes-max 1 --ssh-access --ssh-public-key capstone --managed

11) Create deployment and service yaml 

12) apply deployment and service yaml 
kubectl apply -f deployment.yaml
kubectl apply -f services.yaml

13) Delete a cluster 
eksctl delete cluster -n priyanka-cluster -w

14) To deal with jenkins access issues with docker use below commands
sudo groupadd docker
sudo usermod -aG docker jenkins

Steps to configure jenkins
# Step 1 - Update existing packages
sudo apt-get update

# Step 2 - Install Java
sudo apt install -y default-jdk

# Step 3 - Download Jenkins package. 
# You can go to http://pkg.jenkins.io/debian/ to see the available commands
# First, add a key to your system
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

# # Step 4 - Add the following entry in your /etc/apt/sources.list:
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

# # Step 5 -Update your local package index
sudo apt-get update

# Step 6 - Install Jenkins
sudo apt-get install -y jenkins

# Step 7 - Start the Jenkins server
sudo systemctl start jenkins

# Step 8 - Enable the service to load during boot
sudo systemctl enable jenkins
sudo systemctl status jenkins

Plugins needed 
Blue Ocean
Blue Ocean Executor Info
Blue Ocean Pipeline Editor
Config API for Blue Ocean
Display URL for Blue Ocean
Events API for Blue Ocean
Git Pipeline for Blue Ocean
GitHub Pipeline for Blue Ocean
Pipeline implementation for Blue Ocean
Pipeline: AWS Steps
aws codepipeline 
cloudbees aws credentials 
