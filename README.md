# simple-app-cicd-pipeline



Pipeline using the Jenkins tool as an executor in the CI/CD process. 
Ansible (deploy to kubernetes) is also used helpfully.
In addition, building and testing are performed in the Docker containers.


The event pipeline looks like this: 

1. Download a new version of the code into Jenkins
2. Building a new version of the application
3. Test execution
4. Build docker image and push to DockerHub
5. Deployment to Kubernetes
