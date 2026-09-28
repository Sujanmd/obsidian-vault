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
![[Pasted image 20260925154956.png|490]]


pods- runs one or more closely related containers
services- sets up networking in a kubernetes clusters

![[Pasted image 20260925155256.png|577]]  

Service is used to provide an ip address to a group of pods so that if one pod crashes and a new ip address is created we need not to worry 
unicast multicast, range of ip,192.168