when we install ffmpeg we get 3 things-
![[Attachments/Pasted image 20261016151103.png]] 
-c:v used to set the encoder/decoder library used to compress or decompress video and audio streams.
**Stream Copying (Passthrough):** Using `-c copy` (or `-c:v copy -c:a copy`) copies the audio and video packets directly without decoding or re-encoding. This is instantaneous and causes zero quality loss.

**Time trimming**
- `-ss <time>`: **Start seek** — seeks to a specific timestamp (e.g., `-ss 00:01:30` or `-ss 90`).
    
- `-t <duration>`: **Duration** — limits the output duration to a set length (e.g., `-t 10` for 10 seconds).
in vcodec and acodec, if we pass copy in the place of the encoding scheme, it skips the process.

-r used to set the output frame rate
-f is used to specify the container format
- Usually FFmpeg infers the container from the file extension (`.mp4`, `.mkv`, `.webm`).
    
- `-f` is required when streaming over a pipe/network, writing to stdout, or benchmarking without writing an output file (e.g., `-f null -`).
-pix_fmt Defines the color subsampling and pixel layout (e.g., `yuv420p`, `yuv444p`, `rgb24`).
**Player compatibility:** Most default x264 encoders will pick higher-fidelity subsampling (like `yuv444p` or `yuv422p`) if the source has it, which standard media players (QuickTime, web browsers, mobile devices) cannot decode. Adding `-pix_fmt yuv420p` guarantees universal playback compatibility.
![[Attachments/Pasted image 20261026161125.png]] 
ffmpeg -i output1.ts -c:v libx264 -c:a aac output2.mov
**What it does:** Decodes the video and audio streams from the MPEG-TS container (`output1.ts`), re-encodes video to H.264 (`libx264`), re-encodes audio to AAC (`aac`), and packages them into a QuickTime MOV container (`output2.mov`).

ffmpeg -i output1.ts -map 0:v -c:v copy video.h264 -map 0:a -c:a copy audio.aac

- **What it does:**
    
    - `-map 0:v -c:v copy video.h264`: Extracts only the video stream (`0:v`) from the first input without re-encoding (`copy`) and saves it directly to a raw/elementary Annex B H.264 bitstream file.
        
    - `-map 0:a -c:a copy audio.aac`: Extracts only the audio stream (`0:a`) without re-encoding and saves it as a standalone ADTS AAC elementary stream file.
        
- **Why it's used:** Demuxing container streams into standalone raw media tracks for debugging, muxing tests, or pipeline feeding without quality loss or CPU overhead.

Dump raw frames between 00:01:00 to 00:02:00 (1 minute and 2 minute mark) in a raw yuv file, say tears_of_steel_60s.yuv. -

`ffmpeg -ss 00:01:00 -i output1.ts -t 00:01:00 -pix_fmt yuv420p -f rawvideo tears_of_steel_60s.yuv`

- **What it does:**
    
    - `-ss 00:01:00`: Fast-seeks to the 1-minute timestamp.
        
    - `-t 00:01:00`: Captures 60 seconds of video (from 00:01:00 to 00:02:00).
        
    - `-pix_fmt yuv420p`: Ensures uncompressed pixel layout is planar YUV 4:2:0.
        
    - `-f rawvideo`: Explicitly instructs FFmpeg to strip all headers and container metadata, writing purely raw, uncompressed frame bytes directly to disk.
        
- **Why it's used:** Raw `.yuv` files contain no metadata headers (no width, height, or framerate in the file itself). They are the industry standard input for codec development, algorithmic quality analysis (PSNR/SSIM/VMAF), and raw encoder benchmarking.

`ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 1280x720 -r 24 -i tears_of_steel_60s.yuv -c:v libx264 -crf 23 tears_of_steel_encoded.mp4`

- - `-f rawvideo -pix_fmt yuv420p -s:v 1280x720 -r 24`: **Crucial input parameters.** Because raw YUV files contain no headers, FFmpeg cannot guess the resolution, pixel format, or frame rate. These must be specified _before_ `-i`. (Assuming 720p/24fps matching _Tears of Steel 720p_).
        
    - `-c:v libx264`: Compresses the raw uncompressed frames using the H.264 encoder.
        
    - `-crf 23`: Standard rate control factor for visually lossless balance (default x264 CRF).
        
- **Why it's used:** Demonstrates the process of compressing uncompressed planar pixel buffers into an MP4/H.264 stream.

`ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 1280x720 -r 24 -i tears_of_steel_60s.yuv -c:v libx265 -crf 28 tears_of_steel_encoded.hevc`
- **What it does:**
    
    - Ingests the raw uncompressed YUV stream (`-f rawvideo -pix_fmt yuv420p -s:v 1280x720 -r 24 -i ...`).
        
    - Compresses it using the HEVC/H.265 encoder via `-c:v libx265`.
        
    - `-crf 28`: Sets the Constant Rate Factor. The default visual target for `libx265` is 28 (equivalent in perceptual quality to ~23 in `libx264`, but yielding a roughly 30–50% smaller file).
        
- **Why it's used:** HEVC provides significantly higher compression efficiency compared to H.264, making it standard for high-resolution distribution (4K/8K) or bandwidth-constrained streaming.
Wrap the H264 video from above in a MPEGTS format; call it tears_of_steel_60s.t
`ffmpeg -i tears_of_steel_encoded.mp4 -c:v copy tears_of_steel_60s.ts`
- **What it does:**
    
    - Takes the encoded H.264 video from the previous step as input.
        
    - `-c:v copy`: Uses stream copying (remuxing) to place the existing H.264 video packets into a transport stream (`.ts`) container without decoding or re-encoding.
        
- **Why it's used:** Demonstrates pure container muxing/remuxing. It runs nearly instantaneously with zero CPU load and zero loss of quality.

Play the 1-minute yuv file from the above using ffplay.

`ffplay -f rawvideo -pixel_format yuv420p -video_size 1280x720 -framerate 24 tears_of_steel_60s.yuv`
- **What it does:** Launches the lightweight FFmpeg-based media player (`ffplay`) to render the uncompressed YUV byte stream directly to a display window.
    
- **Why it's used & Gotcha:** Raw YUV files have no header bytes defining width, height, or pixel layout. If you run `ffplay tears_of_steel_60s.yuv` without flags, `ffplay` will default to an incorrect resolution or fail immediately. You must explicitly supply:
    
    - `-video_size 1280x720` (or `-s 1280x720`)
        
    - `-pixel_format yuv420p` (or `-pix_fmt yuv420p`)
        
    - `-framerate 24` (or `-r 24`)

Play the 1-minute ts file from above using ffplay.
`ffplay tears_of_steel_60s.ts`
"Scale the original file to 360p"
ffmpeg -i tears_of_steel_720p.mov -vf "scale=-2:360" -c:v libx264 -crf 23 -c:a copy tears_of_steel_360p.mov
- **What it does:**
    
    - `-vf "scale=-2:360"`: Invokes FFmpeg's video filtergraph. Setting height to `360` scales the vertical resolution down, while `-2` automatically computes the proportional width and ensures it is divisible by 2 (a strict requirement for most H.264/4:2:0 encoders).
        
    - `-c:v libx264`: Re-encodes the scaled frames to H.264 (scaling alters pixels, making re-encoding mandatory).
        
    - `-c:a copy`: Copies the original audio directly without re-encoding to save time and preserve audio quality.
        
- **Why it's used:** Downscaling is fundamental for creating multi-bitrate ladders (ABR streaming like HLS/DASH) and generating lightweight previews or mobile-friendly encodes.

### Comparing Files Using FFmpeg

ffmpeg -i file1.mp4 -i file2.mp4 -i file3.mp4 \
  -filter_complex "[0:v][1:v][2:v]hstack=inputs=3[v]" \
  -map "[v]" -c:v libx264 -crf 18 comparison_output.mp4

# see all the pixel formats!!!


