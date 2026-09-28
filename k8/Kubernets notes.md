Node is single machine either physical server in data center or a vm in cloud that runs docker containers. There are worker node and master node 

![[Pasted image 20260925130823.png|640]]

Master controls the nodes and together the master and node will make cluster

![[Pasted image 20260925131131.png]]

![[Pasted image 20260925131524.png]]

![[Pasted image 20260925131715.png]]

![[Pasted image 20260925143509.png]]

![[Pasted image 20260925143910.png]]

Object is something that exists in the kubernetes cluster

![[Pasted image 20260925145556.png]]
Kind in the yaml file specifies the type of object we want
each api version that we define has a different set of objects in them

![[Pasted image 20260925150115.png]]

Pod is groping of containers , container is embedded in a pod
![[Pasted image 20260925154720.png|487]]

![[Pasted image 20260928120353.png|487]]

pods- runs one or more closely related containers
services- sets up networking in a kubernetes clusters

![[Pasted image 20260925155256.png|577]]  

Service is used to provide an ip address to a group of pods so that if one pod crashes and a new ip address is created we need not to worry 

-unicast multicast, range of ip,192.168
unicast and multicast is one to one and one to many
182.156.94.234- Public ip adddr
10.80.202.5- private ip addr, laptop talks to the router
192.168.49.2- isolated env by minikube, mac can see it but other computers on wifi cannot see it.

Nodeport is used to expose a container to the outside world
The selector spec in one file will act as a reference to the metadata:label:component in the other which is to be refered
selector and metadata should be the same but component:web is like a key value pair which can be anything  
 
-port : is used by any other pod that needs to connect to the multiclient application or the pod that is running
targetPort: send incoming traffic to this port
nodePort: used to test the running container in our browser

![[Pasted image 20260928121938.png|615]]

kubect get services : to get the running services