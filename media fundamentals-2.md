**Interlaced and deinterlaced video**
1. interlaced-
   Instead of capturing and displaying an entire frame all at once, an interlaced video frame is split into two separate halves called **fields**:
   Field 1 (Odd Field):Contains lines 1, 3, 5, 7, etc.
   Field 2 (Even Field):Contains lines 2, 4, 6, 8, etc.
In video and display technology, **"lines"** refer to the **horizontal rows of pixels** that make up a single image or frame.
resolution is defined using this line count, meaning 720p means 720 horizontal line of pixels from top to bottom.
interlacing was developed during the early days of analog television (CRT TVs) as a bandwidth-saving trick:
It allowed broadcasters to deliver 60 motion updates per second (60 fields) using only the transmission bandwidth required for 30 full frames.

2. Deinterlacing- 
   Deinterlacing is the digital process of converting interlaced video into a progressive format (where every frame contains all lines of resolution drawn simultaneously, as denoted by "p" like 1080p).
   complete frames are drawn at once. This requires full bandwidth.

$$\text{Bandwidth} = \text{Pixels per Frame} \times \text{Frames per Second (Updates)}$$
**Resolution**
The total number of pixels horizontally and vertically.
**Bandwidth**
The rate at which digital video data is transmitted over a network. given in bits per second. Mbps or Gbps
**Aspect ratio**
proportional relation between display's width and height.

For audio it is same as video, the multichannel audio is stored in memory as interleaved or planar.

While an **audio channel** represents a single signal path (like Left or Right in a speaker setup), an **audio track** can contain one or more channels bundled together.

Audio track is like the different audio languages that are available, that is the track whereas an audio track can have audio channels inside them, like the kannada track can have 2 channels inside for left and right speakers.

**channel layouts**
while the number of channels tells how many streams exists, the channel layout tells the audio decoder which speaker gets which stream.
eg- mono, stereo, surround

A **sampling rate** (or sample rate) is the number of samples taken per second to convert a continuous analog signal into a discrete digital signal.
The human ear perceives sound up to approximately $20\text{ kHz}$. Therefore, any sampling rate above $40\text{ kHz}$ captures the full spectrum of audible sound.

Audio codec is similar to video codec there are 3 types-
 1. Uncompressed Codecs- .wav
 2. losless codecs- dolby truehd, flac,alac
 3. lossy compressed codecs- mp3,aac,opus

Uncompressed Codecs: No data is discarded or mathematically compressed. They store every raw sample, yielding maximum quality at the cost of huge file sizes.

Lossless Compressed Codecs: Data is mathematically compressed (like a .zip file for audio) without throwing away any acoustic information. When decoded, the output is bit-for-bit identical to the original uncompressed source.

Lossy Compressed Codecs: Psychoacoustic algorithms analyze the audio and permanently delete sounds that human ears are less likely to perceive (such as quiet sounds masked by louder adjacent frequencies). This results in dramatically smaller file sizes.
attenuation is the opposite of amplification

**Audio filtering**
Audio filters alter the frequency spectrum, dynamic range, or temporal properties of a sound wave.
1. Frequency filtering (eq and crossovers):
   low pass filter, high pass filter, band pass ,bandstop, parametric equalizer
2. Dynamic range filtering:
   noise gate- mutes audio that falls below a certain volume
   compressor- reduces the volume of audio that exceeds a threshold
3. Spatial and temporal filtering:
   de-esser- targets and dampens harsh high frequency signals
   echo cancellation- digital filters that subtract speaker output from mic input to prevent feedback in video calls.
**Video filtering**
They work on spatial grid coordinates(pixels within a single frame) or temporal buffers(pixels across sequential frame)
1 spatial filtering:
blurring/smoothing(low pass spatial filter):
gaussan blur to soften image or remove detail, box/median filter to replace each pixel with the median value of surrounding pixel
sharpening and edge detection:
unsharp masking to enhance contrast along edges to make video appear clearer.
2 temporal and spatio temporal filtering(inter frame):
temporal denoising:
compare corr pixels across multiple consec frames. if a pixel fluctuates randomly for a split second it is filtered as noise.
deinterlacing filters:
Reconstructs full progressive frames from alternating interlaced field lines, eliminating combing artifacts on moving objects.
3 colour and pixel value filtering:
colour correction:
Maps input RGB values to target output colors to adjust white balance, exposure, contrast, and cinematic color grading.
chroma keying:
Filters out a specific color range (typically green or blue) to generate an alpha transparency mask for visual background replacement.
De-Banding / Anti-Aliasing: Smoothes harsh color stepping artifacts caused by heavy video compression or low color bit-depths.
![[Attachments/Pasted image 20261015182938.png]] 
$$\text{Bitrate (bits/sec)} = \text{Sampling Rate (Hz)} \times \text{Bits per Sample} \times \text{Channels}$$

$$\text{Data Rate (Bytes/sec)} = \frac{\text{Bitrate (bits/sec)}}{8} = \text{Sampling Rate (Hz)} \times \text{Bytes per Sample} \times \text{Channels}$$

pulse code modulation(pcm)- 
has 3 steps sampling, quantization, binary encoding

**s24le** (Signed 24-bit PCM, Little-Endian)-
s-signed integer which can represent both +ve and -ve audio waveform amplitudes using two's complement.
24- bits(3bytes) per audio sample
le- little endian byte ordering: lsb is stored first in memory access order
uses- x64,x84 desktop processor(intel, amd)

f32be
f-floating point 1 sign bit, 8 exponent bits and 23 mantissa bits, values from -1 to +1
32- 4 bytes per audio sample
be- big endian : msb stored in first memory address 
uses- old network transmission protocols, historical motorola architecture 
