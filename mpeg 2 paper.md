programme- a single broadcast service or channel.
The MPEG-2 Systems Specification1 describes how MPEG-compressed video and audio data streams may be multiplexed together to form a single data stream.

![[Attachments/Pasted image 20261013114749.png]] 
There is no form of error protection within the multiplex.
There is no electrical or physical specification for the MPEG-2 multiplex, so a designer may use the signal levels or connector types that best suit the application.

**Programme stream and transport stream**

programme stream can accommodate single programme only. It is used in error free environments as the stream has succession of long and variable length packets and each begins with the packet header and error in the header causes the whole packet to be dropped and another reason is that if the packet length is varied due to error, the decoder cannot predict the start and end of one packet because it will read from the length field in the header which if lost is gone.

transport stream on the other hand is used in error prone broadcast environment. This multiplex is made for multiprogramme application like broadcasting. it has 188 byte long packet called transport packet. it is not as susceptible to errors as it has short, fixed length packets.

**Presentation units and access units**

Each picture* in its uncompressed form is termed a presentation unit.
The coder compresses each presentation unit to give a coded picture which is termed an access unit.
all access unit are not of the same size, it depends on whether they become i,p or b frames along with how they are coded.
The succession of access units makes the *video elementary stream*.

**packetised elementary streams**

It has pes packets having header and a payload
![[Attachments/Pasted image 20261013123047.png|636]] 


A single pes packet can have many access units. 
PES-packets may be of variable length subject to a maximum length of 64 kBytes. (There is one exception to this: in the case of a video packetised elementary stream when carried in a transport stream, where PES-packets may be of unbounded length.)

![[Attachments/Pasted image 20261013123838.png]]

