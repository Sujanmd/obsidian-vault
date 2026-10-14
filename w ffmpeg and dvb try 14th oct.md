
![[Attachments/Pasted image 20261014124119.png|700]]

Here we can observe that pts is always greater than dts.

**DVB Inspector** is an open-source, Java-based software tool used to analyze captured **MPEG Transport Streams (`.ts` files)**

**TSDuck** is a free, open-source command-line toolkit for manipulating, analyzing, and processing **MPEG Transport Streams**

While both inspect `.ts` files, **DVB Inspector is an offline analytical viewer**, whereas **TSDuck is a live stream processor**.

**`tsanalyze`**
**What it does:** Analyzes a transport stream file or pipe and prints a comprehensive report detailing bitrates, PID distribution, service lists, active tables, and error counts.


![[Attachments/Pasted image 20261014155036.png]] 

here it is bad because, it shows bitrate instability which can cause severe playback stuttering, frame drops, or decoder buffer underflows.

in pcr/pts/dts graph, 
- **Slope (Angle of the line):** Indicates the rate of time advancement. A constant slope means the stream's clock is advancing at the correct speed.
    
- **Vertical Distance Between Lines:** Indicates relative delay or sync gap (e.g., PTS minus DTS = decode buffer delay; Video PTS minus Audio PTS = lip-sync offset).

Decompile raw binary table sections into XML
tsxml sections.bin --output tables.xml

![[Attachments/Pasted image 20261014161529.png]]  

an ideal graph will have a straight slope line and the dist between two lines will be constant throughout

# Pixel format

In any case, all the color coding systems use a group of different values corresponding to precise primaries or properties; they’re called _channels, generally three in number.
- using red, green and blue is close to what the receptor cells in the retina naturally perceive best.
luminance refers to the luminous intensity that is how bright the signal is and chrominance refers to the colors.
![[Attachments/Pasted image 20261014172350.png]] 
- consequently _YUV_ is more widespread in video formats, _RGB_ in image formats.
**Chroma subsampling** is a compression technique that reduces the color detail in an image or video while keeping the brightness detail intact.

The _4:4:4_ subsampling in _YUV_ is **the only equivalent to _RGB_** in terms of quality (and quantity of data)

![[Attachments/Pasted image 20261014183305.png]]

**colour depth (bit depth)**

**Color depth** (also called **bit depth**) defines the number of bits used to represent the color of a single pixel. It determines how many distinct colors, shades, and gradients an image or display can render.
$$\text{Colors per Channel} = 2^{\text{Bits per Channel}}$$
For an RGB image with 3 color channels (Red, Green, Blue):

$$\text{Total Colors per Pixel} = 2^{(\text{Bits per Channel} \times 3)}$$


`4:4:4` (Full Color), `4:2:2` (Broadcast), `4:2:0` (Consumer Streaming).

**pixel format** is simply a **data contract or layout blueprint** that tells a computer CPU or GPU how to read raw binary bytes from memory and turn them into visual pixels on a screen.
