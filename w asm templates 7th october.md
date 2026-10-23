
k3s working, port forwarding and about the templates and the helm chart components

i was not getting the output for the helm chart deployed even though the two containers were running properly because in the command the port number was 8891 and the actual port number was 8890. i got to know about the port number by doing **kubectl describe.**

once we install k3s we get kubectl and k3s , its uses are 
- self healing
- state management and auto scaling
- traffic routing and ingress management
- zero downtime rolling deployments
it creates containers only
it does not use any vm instead runs as a native linux process in server os.
when we do docker ps, we dont get the running pods whereas when we do kubectl get pods, we get the pods that are running, this is because k3s doesnt use docker as the runtime environment, instead it used containerd to run its containers.
statefulsets-
it is a workload api object used to manage stateful applications like db,message queue or distributed storage.
it has sequential 0 indexed pods like db0,db1,... if db0 fails, db0 only will be recreated and not like any random name like deployment

![[Attachments/Pasted image 20261007125317.png]]

underscore in constants.tpl tells helm not to deploy this file directly to k8s as an object instead is a helper template file. This is used to give constant values,urls and other hardcoded values which are not to be changed.
helpers.tpl is used to give the dynamic logic like shutdown timer and cleaning strings,etc
guards.tpl is used to enforce safety rules, block default placeholders and halts deployments if configs are invalid
deployment.yaml is the main execution blueprint in helm chart
podmonitor.yaml is a manifest file that tells promethus operator how to automatically discover and collect metrics from our pods like it tells where to scrape the data to inorder to store the data
configmaps.yaml is used as a key value pair inorder to remove the environment specific working of the container and to inject env variables, dynamic updates and other features.
![[Attachments/Pasted image 20261007151745.png]]

configmap-exporter.yaml is used to specify how system process metrics are gathered

# assembler muxer ui

![[Attachments/Pasted image 20261023152016.png|700]] 
![[Attachments/Pasted image 20261023152054.png]] 
The decode buffer lead is used to show the time by which audio or video data arrives before it is decoded and displayed on the screen , it is useful as it can reduce the jitter or delay that will be there as it can be absorbed
![[Attachments/Pasted image 20261023154029.png]]
This graph displays the **Modeled Eb (Elementary Buffer) Occupancy** for your MPEG-TS stream. It simulates how full the decoder's incoming buffer memory is over time for the video and audio tracks

![[Attachments/Pasted image 20261023154510.png]] 
This is same as the previous one but there the decoded elementary stream will be waiting in the buffer, but here the transport packets land before being filtered

![[Attachments/Pasted image 20261023160058.png]] 
