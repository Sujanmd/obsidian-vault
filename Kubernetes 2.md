![[Attachments/Pasted image 20260928172506.png|507]]
 
![[Attachments/Pasted image 20260929094627.png|598]]

![[Attachments/Pasted image 20260929102452.png|700]] 
Volume- 
generally volume means some mechanism that allows a container to access a filesystem outside itself.
but in kubernetes, volumes is an object that allows a container to store data at the pod level.
![[Attachments/Pasted image 20260929121222.png]]

Deployment->pod->container
k8 volume means that the container can access the data in the volume if the container that is running terminates, ie the volume is specific to the single pod and if the pod dies, the data in the volume is lost.
![[Attachments/Pasted image 20260929121611.png|471]]

so we don't use k8s volume instead we use a persistent volume.
persistent volume is outside the pod and if the pod fails, the new pod will have access to the volume data 

![[Attachments/Pasted image 20260929122002.png|433]]

pvc is like an advertisement for the storage option, once the request is recieved, kubernetes allocates the volume either **statically** or through **dynamic provisioned persistent volume**

![[Attachments/Pasted image 20260929123940.png|520]]

AccessModes- 
![[Attachments/Pasted image 20260929124106.png|549]]

when we have a storage request, in local it slices a part of the hard drive but for cloud provider, if not specified it selects a default persistent storage
![[Attachments/Pasted image 20260929125840.png|485]]
the volume created in the previous code is called in this code as shown above

Secret is a type of object used to securely store, it is imperative and should be manually created.

![[Attachments/Pasted image 20260929144438.png]]

provide environment variables as a string, even port numbers or else there will be errors

LoadBalancer-
it is older way of getting traffic into the cluster it handles all the incoming client requests and makes sure that no server bears too much load, instead we use a ingress service
setting up ingress-nginx will change depending on the environment and we are setting up on local and on google cloud
Ingress-
it is a smart router or reverse proxy that routes external traffic to diff services based on urls or domain paths
loadbalancer is on n/w transport layer,tcp udp
ingress is in application layer, http https 

in k8s a controller is any object that constantly works inorder to make some desired state a reality in our cluster