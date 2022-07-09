# 1. Starting with Pods

## 1. Set Kubernetes Environment

```
  $ eval $(minikube docker-env)
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://192.168.49.2:2376"
    export DOCKER_CERT_PATH="/home/sysadmin/.minikube/certs"
    export MINIKUBE_ACTIVE_DOCKERD="minikube"
  \# To point your shell to minikube's docker-daemon, run:
  \# eval $(minikube -p minikube docker-env)
```

## 2. Build Docker Application image based on shoppingcart

  Create Shoppingcart directory

```
  $ cp -r ../skyblue38-containerize-an-application-lp/shoppingcart/ .
  $ cd shoppingcart
  $ ls
    -rw-rw-r--.  1 sysadmin sysadmin  2294 Jul  9 23:06 app.js
    -rw-rw-r--.  1 sysadmin sysadmin   181 Jul  9 23:06 Dockerfile
    -rw-rw-r--.  1 sysadmin sysadmin    68 Jul  9 23:06 .dockerignore
    -rw-rw-r--.  1 sysadmin sysadmin  1024 Jul  9 23:06 items.json
    drwxrwxr-x. 69 sysadmin sysadmin  4096 Jul  9 23:06 node_modules
    -rw-rw-r--.  1 sysadmin sysadmin   366 Jul  9 23:06 package.json
    -rw-rw-r--.  1 sysadmin sysadmin 20101 Jul  9 23:06 package-lock.json
    drwxrwxr-x.  3 sysadmin sysadmin    90 Jul  9 23:06 public
    drwxrwxr-x.  2 sysadmin sysadmin    23 Jul  9 23:06 views
  $ cat Dockerfile
    FROM node:14-alpine
      WORKDIR /usr/src/app
      COPY package*.json ./
      RUN npm install
      RUN npm audit fix
      RUN apk update && apk add vim
      COPY . .
      EXPOSE 5000
      CMD ["node", "app.js"]
```

  Build Docker image


```
$ docker build -t scart:1.1 .
Sending build context to Docker daemon  611.8kB
Step 1/9 : FROM node:14-alpine
14-alpine: Pulling from library/node
2408cc74d12b: Pull complete
2575f741156b: Pull complete
251fa4f1a69d: Pull complete
b2cd4a2d8991: Pull complete
Digest: sha256:2af507df45e7c0a46c6b3001ce0dbc6924f7b39864d442045f781361a1971975
Status: Downloaded newer image for node:14-alpine
 ---> 39e98037c459
Step 2/9 : WORKDIR /usr/src/app
 ---> Running in 5e6ea16870c3
 ---> e45b55b710de
Step 3/9 : COPY package*.json ./
 ---> 08a3af3665d4
Step 4/9 : RUN npm install
 ---> Running in 54ed58b9a05b
added 69 packages from 93 contributors and audited 69 packages in 5.206s
found 0 vulnerabilities
 ---> 16b66e79f309
Step 5/9 : RUN npm audit fix
 ---> Running in 88862531c817
up to date in 1.229s
fixed 0 of 0 vulnerabilities in 69 scanned packages
 ---> 6b006c77864b
Step 6/9 : RUN apk update && apk add vim
 ---> Running in 9f12920e63f4
fetch https://dl-cdn.alpinelinux.org/alpine/v3.16/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.16/community/x86_64/APKINDEX.tar.gz
v3.16.0-286-g30edee2642 [https://dl-cdn.alpinelinux.org/alpine/v3.16/main]
v3.16.0-293-gce7fda3927 [https://dl-cdn.alpinelinux.org/alpine/v3.16/community]
OK: 17030 distinct packages available
(1/5) Installing xxd (8.2.5000-r0)
(2/5) Installing lua5.4-libs (5.4.4-r5)
(3/5) Installing ncurses-terminfo-base (6.3_p20220521-r0)
(4/5) Installing ncurses-libs (6.3_p20220521-r0)
(5/5) Installing vim (8.2.5000-r0)
Executing busybox-1.35.0-r13.trigger
OK: 37 MiB in 21 packages
 ---> 3ae296afdefe
Step 7/9 : COPY . .
 ---> c5f75819242d
Step 8/9 : EXPOSE 5000
 ---> Running in 4ed74023d685
 ---> 152425da6d08
Step 9/9 : CMD ["node", "app.js"]
 ---> Running in faf0b9aef204
 ---> 922471c61aa8
Successfully built 922471c61aa8
Successfully tagged scart:1.1
```

  List available images

```
$ docker images
REPOSITORY                                TAG         IMAGE ID       CREATED         SIZE
scart                                     1.1         922471c61aa8   4 minutes ago   156MB
node                                      14-alpine   39e98037c459   19 hours ago    119MB
```

## 3. Create Kubernetes Pod Running Shoppingcart


```
$ kubectl run scart --image=scart:1.1 --image-pull-policy=Never
  pod/scart created
$ kubectl get pods
  NAME    READY   STATUS    RESTARTS   AGE
  scart   1/1     Running   0          25s
$ kubectl port-forward --address 0.0.0.0 pod/scart 5000:5000
```

## 4. View running application in a Windows Browser

  Open firefox on Windows desktop and go to URL http://192.168.0.81:5000/store

  Browser displayed:

![scart Store running screenshot](https://github.com/skyblue38/MLP-875-Deploy-an-Application/blob/main/shoppingcart/MLPstore.png)

