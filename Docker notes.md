Docker client is the tool used to issue commands (docker cli)  
Docker server is responsible for creating images, containers (docker daemon)  
Docker hub is like GitHub having free images  
  
![FS Snapshot](Attachments/47DC1723-BC08-4B64-88D4-122802ADC255.png)  
  
![Creating and Running a](Attachments/2BF6945D-9958-4683-A0CC-B02D62FB4BD0.png)  
  
  
* Docker ps- to list all the running containers  
* Docker ps—all - to list all the containers that were created  
* Docker run= docker create +docker start  
* When we do docker create it gives the id of the container created on the terminal and the same outputs if we give “docker start id” so we need to give ‘docker start -a id’ to get the output on the terminal  
* Docker system prune- delete all the containers that were present  
* Docker logs id- to print all the logs that were there even in the closed containers   
* Docker stop and docker kill is used to terminate a running container but there is diff stop is graceful shutdown whereas kill is aggressive  
* Docker exec used to execute additional command in a container  
* Docker exec -i and -t together -I means attach to standard in of cli -t makes it pretty (format)  
* Docker run -it busy box sh - to go into shell mode  
  
  
  
* ![Execute an additional](Attachments/21AFA423-A057-471C-81FC-8DC51FE8519E.png)  
  
* ![Creating a Dockerfile](Attachments/3FF12B40-62F6-4157-B70D-428D0EA0D61C.png)  
* ![Instruction telling](Attachments/04D3DEC9-C4BC-4B99-BE3A-D1839BBB0EEC.png)  
  
  
* Docker build is like compiling the text written in Dockerfile to make it like a ready frozen meal which can be run later  
* Inorder to tag an image we use “docker build -t sujanmd/redis-server:latest .”. Once this is done we can just run docker run sujanmd/redis-server  
  
  
To do port mapping we use “docker run -p 8080:8080 image name”  
  
![Docker Run with](Attachments/D126390B-DDC5-45CD-B3C9-770ACFD3DDB2.png)  
  
  
![base image](Attachments/F1ADF892-898C-4B88-8AA4-B1EF1B2D5D28.png)  
  
![FROM node: alpine](Attachments/C205E0C6-D64B-41AD-BA37-38B58C766973.png)  
Here the first copy line says that the package.json file is in the local Mac system and is copied to the container workspace and the second copy says that the remaining codes that are there in the local folder is copied   
  
  
* Docker save is used to package a docker file into a .tar archive  
* Docker load is used to do the reverse of docker save or simply unpack the tarball  
* Docker export on the other hand targets a container and takes a snapshot of the current filesystem into .tar file  
Transfer can be done using scp itself or using any ftp agent   
  
  
![Docker Compose](Attachments/D37FA930-79D2-45F6-8A8F-92E965AD38F3.png)  
  
- In yaml file is used to specify array   
* Docker compose is used to remove the work of specifying long docker run commands and also gives the option of communication between multiple containers  
* Docker-compose up is used to run an image that is already present whereas docker-compose up —build is used to freshly rebuild the image and then run it  
* ![Launch in background](Attachments/8A253E63-21A1-4789-9F72-B7C3CE41EB1F.png)  
  
![Restart Policies](Attachments/C6D8BA18-F8F4-4CC1-87B4-29E867870EE1.png)  
  
* If in case a container fails and we want to restart it there are these restart policies  and the code is written in docker-compose file   
* Use -d flag for detached mode so that the container is built in the background and the cli can be used   
* ![version: '3'](Attachments/67A69728-BE4F-43C5-8C1F-484CDC7F8601.png)  
  
* When we want hot reload we use Dockerfile.dev  
* ![version: '3'](Attachments/734A0D49-FBD9-4562-A4E3-D17AE3AB83F0.png)  
  
HERE, the second line with the colon says that copy everything inside the server folder to the container workspace except the node_modules which is specified in the first line.  
* The Postgres and redis services don’t have build because they just pull the image from dockerhub and run it directly  
* Context is used to specify the code directory   
* Volumes is expected to have a list format so - is used before  
  
![1 Mersion:](Attachments/2EA4B1A9-C8AC-468F-9830-5173F3947D3C.png)  
  
* Nginx is used as a reverse proxy , it is a web server. It is used to map the endpoint calls to the server efficiently. Like if the call request has /api/ then it redirects to the express server or else to the react server  
