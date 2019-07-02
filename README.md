Final API which can be called from any terminal: "curl -X POST -F image=@dog.jpg 'http://35.188.83.248/predict'"

# Resnet-Flask-Kubernetes-GCP
Deploying a pre-trained Keras Resnet model with Flask on GCP Kubernetes


Create an instance 
- n1-standard-4 (4 vCPU, 15 GB memory)
- Allow HTTP and HTTPS traffic
- In boot disk option choose CentOS 7
- And increase boot disk size to 100 GB

SSH into the compute VM
- Delete existing docker if any 
"sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine"
- Install docker 
 "sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2" 
 "sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo"
 "sudo yum install docker-ce docker-ce-cli containerd.io"
 "sudo systemctl start docker"
- To test everything is working fine
"sudo docker run hello-world"

Get the relevent files
- Visit https://github.com/omar16100/Resnet-Flask-Kubernetes-GCP
- "mkdir keras-app"
- "cd keras-app"
- "vim app.py" 
- Copy the contents of app.py. Press i, paste using CTRL+C. Press ESC. Write ':x' to exit.
- "vim requirements.txt"
- Copy the contents of requirements of requirements.txt. Same procedure.
- "vim Dockerfile"
- Copy the contents of Dockerfile. Same procedure.

Build docker container 
- "sudo docker build -t keras-app:latest ."

Run docker container
- "sudo docker run -d -p 5000:5000 keras-app"

Check status
- "sudo docker ps -a"

Create Docker Hub account

Connect with Docker Hub account from the VM
- "sudo docker login"
- Provide details as required

Tag the keras-app image
- "sudo docker images" to find image ID (keras-app image size > 1.5 GB)
- #Format
"sudo docker tag <your image id> <your docker hub id>/<app name>"
- #My Exact Command - Make Sure To Use Your Inputs
"sudo docker tag 1fa88daaa455 omar16100/keras-app"
  
Push container to Docker Hub
- #Format
"sudo docker push <your docker hub name>/<app-name>"
- #My exact command
"sudo docker push omar16100/keras-app"

Create Kubernetes cluster on GCP
- 4 vCPUs
- Number of nodes : 3

Wait for the clsuter to create

SSH into the cluster by clicking 'Connect' -> 'Run in Cloud Shell'
Run the first command after cloud shell starts.

Run our docker constiner in Kubernetes by
- "kubectl run keras-app --image=USERNAME/keras-app --port 5000"

Expose port 80 so that external traffic can access the API
- "kubectl expose deployment keras-app --type=LoadBalancer --port 80 --target-port 5000"

Determine status
- "kubectl get service"
Wait for the EXTERNAL-IP

Make an API call from any terminal
- #FORMAT curl -X POST -F image=@dog.jpg 'http://<your service IP>/predict' 
"curl -X POST -F image=@dog.jpg 'http://35.188.83.248/predict'"
Expected terminal output = '{"predictions":[{"label":"beagle","probability":0.987775444984436},{"label":"pot","probability":0.0020967808086425066},{"label":"Cardigan","probability":0.001351703773252666},{"label":"Walker_hound","probability":0.0012711131712421775},{"label":"Brittany_spaniel","probability":0.0010085132671520114}],"success":true}'

You can try with different photos.

References 
- https://docs.docker.com/install/linux/docker-ce/centos/
- https://github.com/jrosebr1/simple-keras-rest-api
- https://medium.com/analytics-vidhya/deploy-your-first-deep-learning-model-on-kubernetes-with-python-keras-flask-and-docker-575dc07d9e76
