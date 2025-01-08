# remove current Ubuntu from windows (optional)
wsl --unregister Ubuntu-22.04

sudo apt-get update
sudo snap install kubectl --classic
sudo apt-get install docker.io -y
sudo apt install curl git python3 python3-pip -y
sudo apt  install ffmpeg -y
sudo apt install timg -y
sudo apt install vlc -y

# install kind
curl -Lo ./kind "https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64"
chmod +x kind
sudo mv kind /usr/local/bin/
sudo kind create cluster
sudo snap install minikube

# install minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# install docker & add current user to it
sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker $USER
sudo usermod -aG docker $USER && newgrp docker


sudo docker run hello-world

kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml


minikube delete              # Clean up any broken state
minikube config set driver docker
minikube start

# hard to copy to WSL, so I used temp git repo
git clone https://github.com/elliott3/rtmp
cd rtmp

# change ffmpeg source in ffmpeg-deployment.yaml

# terminal
      ffmpeg -i rtmp://liteavapp.qcloud.com/live/liteavdemoplayerstreamid
      -c:v libx264 -preset veryfast -crf 28
      -c:a aac -strict experimental -b:a 128k
      -f hls -hls_time 4 -hls_list_size 10 -hls_flags delete_segments
      /shared/index.m3u8


kubectl apply -f pv-pvc.yaml
kubectl apply -f ffmpeg-deployment.yaml
kubectl apply -f nginx-deployment.yaml
kubectl apply -f ffmpeg-service.yaml
kubectl apply -f nginx-service.yaml

# get the nginx-pod name
kubectl get pods

# get the ip of nginx-pod
kubectl describe pods <nginx-pod>

# <nginx-ip> looks like this ==> Node: kind-control-plane/xxx.xx.x.x>

curl http://<nginx-ip>:30000/live/index.m3u8

# you will see .m3u8 and .ts file

