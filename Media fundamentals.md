mpeg- moving picture experts group
in mpeg, a programme is a channel or a single broadcast service
codec- a software that shrinks the raw video and audio so it takes less space
container- file wrapper that holds the compressed video,audio and extra data together in a single file.eg- mp4,mkv,mov,webm.
steps-
1. first audio and video is compressed by using codecs and the software used to do so is encoder.
2. then the audio and video and chopped into small pieces and interleaved together into a single stream, complete with timestamps to keep audio and video in sync. This is done by a **multiplexer** .There are many ways the media is packaged, like mp4 or mkv
3. once packaging is done, transport protocols carry the data accross networks like http,tcp or rtp,udp.
4. demultiplexing and decoding at the destination is done later , demux unwraps the container and seperates the interleaved packets back into audio,video and subtitile streams, decoder uncompresses the codec streams back into raw frames and audio samples and finally renderer use the embedded timestamps to play the video on screen and output audio simult.
mux uses chunking and interleaving where audio and video is broken down into small packets and instead of putting all video first then audio, it alternates them. 
time-stamping- each packet gets a timestamp(presentation time stamp-PTS). this tells to play video frame 30 at same millisecond as audio frame 30.
multiplexing is combining audio, video and subtitle into one single file and demux is the opp.

![[Attachments/Pasted image 20261012144730.png]]

TS- Transport stream mxf- material exchange format

TS is designed by mpeg for broadcast transmission over error-prone media and live streaming

![[Attachments/Pasted image 20261012150605.png]]


mxf designed by smpte-Society of Motion Picture and Television Engineers
for professional broadcast, post-production and digital archive workflows

![[Attachments/Pasted image 20261012154725.png]]

ES- elementary stream
is the raw, continuous output stream produced directly by an encoder for single media type(video, audio or subtitiles)
PES- packetized elementary stream
es is broken into pes packets which append timing information
PTS(presentation time stamp) DTS(decode time stamp)-
pts tells the display engine when to render a frame on screen and dts tells decoder engine when to decode a frame from the buffer.
codec- is the algorithm used to compress raw spatial pixels and temporal movement into reduced binary footprint 

h264 is also known as AVC (advanced video coding)-
it uses 2 facts
spatial redundancy- nearby pixels in the same image frame often look almost identical
temporal redundancy- most of the consecutive frames are identical

interlaces(i)- frames are split into alternating fields like odd and even and are captured at seperate time intervals
progressive(p)- frames are rendered top-to-bottom sequentially 
timebase=1/ clock frequency 

I-frame
i frame or a key frame or intra-frame consists only of macroblocks that use intra-prediction , it can only use "spatial redundancies" 

P-frame
p-frame means predicted frame and allows macoblocks to be compressed using temporal prediction in addition to spatial prediction. for motion estimation, they use frame that have been previously encoded

![[Attachments/Pasted image 20261012170010.png]]

B-frame
b-frame refers to bi-directional frame and as they can interpolate from 2 or more frames that occur before and after it, b frames can be efficient in reducing the size of the frame while retaining video quality.But they are resource heavy both at the encoder and the decoder
i frame is used to refresh video quality and are usually added at the end of a gop as they don't depend on any other frames.
GOP(group of pictures)-
defines the exact arrangement and display order of iframes, pframes and bframes.
gop length(keyframe interval)- the total no of frames between two consecutive i-frames N
gop structure- distance b/w 2 anchor frames (I or P).
closed gop- no frames inside the gop can reference frames outside it.
open gop- b frames at the beginning of the gop are allowed to reference frames in the previous gop.
n,m and open or closed gives the structure of gop.
gop applications-
1. adaptive bitrate streaming- web streaming breaks video into smaller chunks so that player can switch resolutions mid stream based on internet speed. every chunk starts with i frame
2. low latency live stream- zoom calls, security cameras. short gop and only intra frame because b frames require waiting for future frames to decode, adding delay so b frames should be removed inorder to facilitate no delay. And quick recovery will be there if shorter gop is there(less than a second).
3. video editing and post production- adobe premier pro, davinci resolve. all i frame of size 1 as editor need to cut video precisely on every single frame.
4. media storage- offline videos uses long gop and heavy b frame usage as storage space efficiency is the top priority and p and b frames takes less data than i frames.

