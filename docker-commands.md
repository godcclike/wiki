# Docker Commands
```Shell
docker build -t <image name> -f <dockerfile>
or
docker build -t <image name> .

docker build -t oms-fix-server -f Dockerfile-8-fix-server .

docker images                    (to check which images is in your local repo)
docker image rm <image id>       (to remove and image)
docker image prune -f            (remove all untagged images)

docker ps -a                     (to check all images in your local pc)

docker tag <image name:latest> <url:port number or ECR>/<image name>:latest      (tag the image)
docker tag ms-pats-oms-ah-adapter:latest 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/ms-pats-oms-ah-adapter:latest 

docker push <url:port number or ECR>/<image name>:latest     (to push the image to a remote repo)

docker image pull <url:port number or ECR>/<image name>      (to pull the image to a remote repo)

sudo systemctl daemon-reload       #restart docker daemon
sudo systemctl restart docker         #restart docker service

docker run <image name>    #to run the docker image manually

tail /var/log/docker.log    #to check docker logs
docker logs <container ID>  #to check docker container logs
```

## Loging in to a remote repo
```Shell
docker login -u $nexusUser -p $password $nexusUrl
docker login –u admin –p Welcome1 192.168.0.24:8888

or 

aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com
```

## Create a container and going inside it
```Shell
docker build -t <image name> .
docker run –-name <container name> -itd <name>
docker exec -it <container name> bash

docker build -t <image name> .
docker run --name platinum -itd platinum
docker exec -it c1 bash

docker rm -f <container name>

```

## Start a container
```Shell
docker ps -a
docker start <container name/id>
docker stop <container name/id>

```

## For ECN
```Shell
docker load -i <filename.tar.gz>
docker load -i ecn_dbs_prod_image.tar.gz

docker images  (to get the image id)
docker ps -a   (to get the container id)

docker run --name <image name> -itd <image ID>
docker run --name ecn_dbs -itd 66ec55df389c

docker exec -it <container ID> bash
docker exec -it 8737bf49e179 bash

docker rm -f <container name>
docker image rm <image id>
```