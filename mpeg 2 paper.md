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

stream_id byte- distinguishes the pes packets belonging to an elementary stream from those of another within the same programme. 
flags are 'indicator bits' which show the presence or absence of various optional fields that may be included in a pes packet header. they give additional info like whether the stream is scrambled or not, relative priority, copyright info and optional error-check field.
flags 2 have the pts and dts ie they indicate their presence.
![[Attachments/Pasted image 20261013130540.png|700]]

A transport packet is always 188 bytes long. 

![[Attachments/Pasted image 20261013140341.png]] 
a single transport stream may carry different programmes, each comprising many packetised elementary streams. The 13 bit **packet identifier**(pid) is used to distinguish transport packets containing the data of one elementary stream from the other elementary stream

Programme specific information(PSI)- helps the decoder to determine which elementary stream its payload belongs to. They have types-
1. Programme map table(PMT)
2. Programme Association Table(PAT)
3. Network information table(NIT)
4. Conditional Access Table(CAT)

**PMT**

![[Attachments/Pasted image 20261013142613.png]] 

pmt is used to locate the video pid and audio pid then demux the correct audio and video streams.
it is also used when there are multiple audio and subtitle selection

**PAT**
![[Attachments/Pasted image 20261013145219.png|553]] 

A complete list of all the programmes available in a transport stream is maintained in the Programme Association Table.
This table is easily found as it always has the PID value 0.

**NIT**

the programme number 0 in pat is the nit it is used to provide info about channel frequencies, satellite transponder details, modulation characteristics, service originator, service name and details of alternative networks available.

**CAT**
scrambling means encrypting the audio and video so that users who have not payed for the service cannot see the service.
cat table contains the info about the encryption so that the set top box can decrypt it.
it has a fixed pid 1.
cat points to encrypted keys for scrambled streams.
![[Attachments/Pasted image 20261013151535.png|669]] 

