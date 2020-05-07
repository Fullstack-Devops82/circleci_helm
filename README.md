# Helm Chart with CircleCI
You can deploy the Deployment, Pod and Services on AWS EKS using Helm Chart.
# Installation
### Clone Git Repository
```sh
$ git clone https://github.com/kubix-io-ltd/nazar.git
```
### Edit Dockerfile
You have to edit the Dockerfile.
For example
```sh
FROM ubuntu:latest

RUN apt-get update \
    && apt-get install -y nginx \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "daemon off;" >> /etc/nginx/nginx.conf

VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

WORKDIR /etc/nginx

EXPOSE 80
CMD ["nginx"]
```
### Build Docker image and Push to Repository
For example, this build and push the image to AWS ECR.
```sh
$ docker build -t <Account-ID>.dkr.ecr.<Region>.amazonaws.com/<Repository-Name>:<Tag> .
$ docker push <Account-ID>.dkr.ecr.<Region>.amazonaws.com/<Repository-Name>:<Tag>
```
### Edit Chart.yaml file
You must change a chart name in the file and must use the chart name when install helm chart.
For example
```sh
name: application
```
You can change some version such as appVersion.
For example
```sh
appVersion: <Image-Tag>
```
### Edit .circleci/config.yml file
Please replace the repository, cluster and chart name in the file.
### Edit values.yaml file
You can change some values such as image/repository in the file.
For example
```sh
image:
  repository: <Account-ID>.dkr.ecr.<Region>.amazonaws.com/<Repository-Name>
```
### Install your chart using Helm
```sh
$ helm install application .
```
### Deploy a service with LoadBalancer type
```sh
$ cat values.yaml
...
service:
  type: LoadBalancer
...
$ helm upgrade application .
```
### Deploy a service with ClusterIP type
```sh
$ cat values.yaml
...
service:
  type: ClusterIP
...
$ helm upgrade application .
```
