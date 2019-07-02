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
To test everything is working fine
"sudo docker run hello-world"

- Get the relevent files 


References 
- https://docs.docker.com/install/linux/docker-ce/centos/
- https://github.com/jrosebr1/simple-keras-rest-api
- https://medium.com/analytics-vidhya/deploy-your-first-deep-learning-model-on-kubernetes-with-python-keras-flask-and-docker-575dc07d9e76
