# capstone
Capstone project

Git HUb repo : https://github.com/peyushjain1/capstone

1) Github repo contain following 
 - Capstone.txt  - Contain all steps to create kubernetes cluster. 
 - Jenkins.txt   - COntain all steps to setup Jenkins 
 - Jenkinsfile   
 - Dockerfile 
 - index.html - Code used in docker 
 - cluster networking.yml  (created by eksctl)
 - cluster worker nodes.yml (created by eksctl)
 - deployment manifest 
 - servcies manifest 
 - Screenshot : Lint error in Jenkins
 - Screenshot : Successful Jenkins pipeline
 - Screenshot : Successful deployment from Jenkins
2) I have used eksctl to create kubernetes cluster. ALl the steps related to that are present in text file named 'Capstone.txt'.
3) eksctl automatically created all networking related to a kubernetes cluster and worker nodes via cloud formation. Yaml used by same is copied and is added to git hub repo. 
4) All steps to setup Jenkins are present in 'Jenkins.txt'
