
- server:   amagi@10.0.9.68
- pw-  hlWJ2I86+:Xy
- we need to use CMD ["chrt","-r","99","./a.out"] so that we get the priority of the thread to be max and to execute it in round robin fashion
- The **`--cap-add=sys_nice`** flag stands for **"Capability Add: System Nice."**
  this is used before docker run so that it makes our process nice and it gives time to others
- ![[Attachments/Pasted image 20260930154928.png|559]]
- docker run --cap-add=sys_nice -it <your-image-name> this is the syntax
- RUN apt-get update && apt-get install -y \ gcc \ && rm -rf /var/lib/apt/li 

System nice is security capability that grants the program the authority to alter process execution priorities 
more about sysnice and to give input through helm
when we give sysnice to a container or a process it gives the foll:
1. power to use real time scheduling
2. give itself high priority over others
3. can change the priority of other processes
4. bind to specific cpu cores










